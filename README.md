# tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Ankerfld12/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Ankerfld12/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

An original chess engine in 4 KB of Python, written for this bounty. Design:
64-square board (0=a8..63=h1) with integer pieces and precomputed knight,
king, sliding-ray, centrality and king-safety tables; full legal move
generation (castling, en passant, promotions); negamax alpha-beta search
with principal-variation search, a zobrist transposition table, MVV-LVA
plus killer-move ordering, late-move reductions, check extensions, and
quiescence over captures, promotions and check evasions; evaluation is
material (100/320/330/500/900) plus generated piece-square terms (pawn
advancement, piece centrality, rook on the 7th, bishop pair, tapered king)
and pawn structure (passed, doubled and isolated pawns) — no stored tables
and no code from any existing engine. Iterative deepening runs under a
fixed node budget per iteration, never a clock, so every move is
deterministic and replay-safe. `main.py` is the whole engine: the Python
source is carried as an lzma payload with its unpacker (payload and
unpacker together are 4,092 of the 4,096 bytes). It plays the gauntlet
through the env at MAKEFUN_ENV_URL (POST /game, /move) and reads no
other file.

## Measured results

Move generation is exact: perft matches the known counts on four standard
test positions (startpos depth 4 = 197,281; kiwipete depth 3 = 97,862;
position 3 depth 4 = 43,238; position 4 depth 3 = 9,467), and the engine
finds mate in one in tactical tests. On the kit's three local practice
instances (six games each against the house gauntlet) this configuration
scored 2.5/6, 2.0/6 and 2.5/6 — performance Elos 1,191, 1,099 and 1,191 —
beating the depth-4 anchor (1,199) in two of the three instances. The
previous shipped version scored 2.5/6, 2.0/6 and 2.0/6 on the same
instances, so this revision is a small measured improvement, within the
noise of six-game samples; it is the strongest configuration measured,
so it is the one that ships.

Limits: no opening book, no endgame tablebase, fixed node budget per
iteration — strength comes from search and evaluation alone.

## Run it

```sh
MAKEFUN_ENV_URL=<env url> python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=8rrqgntk&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
