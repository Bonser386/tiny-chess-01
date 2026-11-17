# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Bonser386/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Bonser386/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in one Python file (4,055 bytes, stdlib only) for the "strongest chess engine in 4 KB" bounty. 0x88 board, full legal move generation (castling, en passant, promotion), negamax alpha-beta with iterative deepening under a fixed node budget (120,000 nodes/move), null-move pruning, quiescence, transposition table, tapered king table. The file is the engine source packed with lzma (allowed by the rules; payload + unpacker count toward the 4,096-byte cap), the same packing the house sunfish baseline uses.

Move generation matches python-chess perft exactly on 8 test positions; 30 random games match after every move. Local practice trials vs the real house gauntlet: trial-0 4.0/6 (Elo 1,451), trial-1 2.5/6 (Elo 1,191), mean ~1,321 vs sunfish baseline mean 1,365. No illegal moves or timeouts in 700+ moves; slowest move 6.9s (limit 10s).

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=7dkxfn6v&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
