# Tiny Chess — make.fun bounty tiny-chess-01

A chess engine in one Python file (4,055 bytes, stdlib only) for the
"strongest chess engine in 4 KB" bounty. It plays the house gauntlet
through the env at `MAKEFUN_ENV_URL`.

## How it works

- 0x88 board, full legal move generation (castling, en passant, promotion)
- Negamax alpha-beta with iterative deepening under a fixed node budget
  (120,000 nodes/move — deterministic, no clock), null-move pruning,
  check extensions, quiescence on captures with check evasions,
  transposition table, killer moves, history ordering
- Evaluation: material + piece-square tables, tapered king table
  (middlegame safety blended into endgame centralisation by game phase)
- Protocol: `POST /game {}`, `POST /move {"game": n, "move": "<uci>"}`,
  loop until the env answers `status: "done"`

The file is the engine source packed with lzma (allowed by the rules;
payload + unpacker count toward the 4,096-byte cap), the same packing
the house sunfish baseline uses.

## Run

```
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

## Tests

- Move generation matches python-chess perft exactly on 8 test positions
  (start, Kiwipete, en passant, promotions, castling) to depth 3
- 30 random games: board state matches python-chess after every move
- Local practice trials vs the real house gauntlet (official kit env):
  - trial-0: 4.0/6 — beat depth-2, depth-3, depth-4 and nodes-100k,
    lost to nodes-250k and nodes-500k. Practice Elo 1,451 (sunfish
    baseline mean 1,365). Wall 18.4 min, slowest move 6.9s (limit 10s)
  - trial-1 (colours swapped): 2.5/6 — beat depth-2, drew depth-4,
    nodes-100k and nodes-250k, lost to depth-3 and nodes-500k.
    Practice Elo 1,191. Wall 22.4 min, slowest move 6.9s
  - Two-trial mean Elo ~1,321 vs sunfish's three-trial mean 1,365:
    roughly at baseline level, not clearly above it. Single-trial
    Elo is noisy (sunfish's own trials range 1,253–1,396), and the
    final also adds the nodes-1M and nodes-2M rungs, which this
    engine (like sunfish) loses to. No illegal moves or timeouts
    in any trial (700+ moves played).
- Earlier configs tested on trial-0 for comparison: plain engine
  100k nodes 3.5/6 (Elo 1,365); 150k nodes + null-move + extensions
  2.5/6 (Elo 1,191). The shipped config (120k nodes + tapered
  endgame king table) was the best of the three.
