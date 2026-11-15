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
unpacker (payload and unpacker together are 3,873 of the 4,096 bytes). It plays the gauntlet through the env at
MAKEFUN_ENV_URL (POST /game, /move) and reads no other file.

## Measured results

Verified locally before shipping. Move generation is exact: perft matches
the known counts on four standard test positions (startpos depth 4 =
197,281; kiwipete depth 3 = 97,862; position 3 depth 4 = 43,238; position
4 depth 3 = 9,467), and the engine finds mate in one and wins a hanging
queen in tactical tests. Final `main.py` is 3,873 of the 4,096 bytes
allowed. In a full practice trial on the kit's own environment (six games
against the house gauntlet) the shipped configuration scored 2.5/6 — wins
by checkmate over the depth-2 and depth-3 anchors and a draw against the
500k-node anchor — for a performance Elo of 1,191: above the 1,154 floor
for the staged final, below the 1,365 practice mean of the sunfish
baseline. Two stronger-on-paper revisions were measured on the same trial,
then rejected on the data: adding killer-move ordering, a 20,000-node budget and null-move
pruning scored 2.0/6 (Elo 1,099), and the same revision without null-move
pruning also scored 2.0/6 (Elo 1,099), so the simpler 12,000-node
configuration is the one that ships.

Limits: no opening book, no endgame tablebase, and a fixed node budget per iteration — strength comes from search and evaluation alone, so play is weakest in quiet endgames and against deep preparation.

## Run it

```sh
MAKEFUN_ENV_URL=<env url> python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=8rrqgntk&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
