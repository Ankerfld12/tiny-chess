# tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Ankerfld12/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Ankerfld12/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

An original chess engine in 4 KB of Python, written for this bounty. Design:
64-square board (0=a8..63=h1) with integer pieces and precomputed knight,
king, sliding-ray, centrality and king-safety tables; full legal move
generation (castling, en passant, promotions); negamax alpha-beta search
with a zobrist transposition table, MVV-LVA move ordering, check
extensions, and quiescence over captures, promotions and check evasions;
evaluation is material (100/320/330/500/900) plus generated piece-square
terms (pawn advancement, piece centrality, rook on the 7th, bishop pair,
tapered king) — no stored 6x64 tables and no code from any existing engine.
Iterative deepening runs under a fixed node budget per iteration, never a
clock, so every move is deterministic and replay-safe. `main.py` is the
whole engine: the Python source is carried as an lzma payload with its
unpacker (a form the rules expressly allow; payload and unpacker together
are 3,873 of the 4,096 bytes). It plays the gauntlet through the env at
MAKEFUN_ENV_URL (POST /game, /move) and reads no other file.

## Run it

```sh
MAKEFUN_ENV_URL=<env url> python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=8rrqgntk&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
