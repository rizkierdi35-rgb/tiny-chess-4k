# Tiny Chess — 4 KB Python Chess Engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/rizkierdi35-rgb/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/rizkierdi35-rgb/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Entry for the make.fun bounty **"Build the strongest chess engine in 4 KB of Python"** ($500, deadline 24 Nov 2026 17:00 UTC) — see [the bounty on make.fun](https://make.fun/bounty/tiny-chess-01).

## What it is

`main.py` is a complete chess engine in 3,346 bytes: 0x88 board, alpha-beta
with quiescence, iterative deepening to a fixed 60,000 nodes per move,
transposition table, null-move pruning, killer moves, history heuristic,
LMR (with re-search), MVV-LVA move ordering, and formula piece-square
tables with an endgame king table. No clock, no unseeded randomness —
fully deterministic (verified: two fresh processes play the same move on
the same position). Stdlib only. The readable source (`v4.py`, 7,899 bytes)
is packed with the kit's `makefun compress` into `main.py`.

It talks to the make.fun environment at `MAKEFUN_ENV_URL`: it asks for the
game in play, replays the moves, and sends its reply in UCI.

## Files

- `main.py` — the engine (3,346 bytes, packed from `v4.py` by the kit's
  `makefun compress`; the packed payload decompresses byte-for-byte to `v4.py`)
- `LICENSE` — MIT

## Local check

```sh
python3 main.py   # needs MAKEFUN_ENV_URL set
```

## Local test results (v2, exact kit-packed file)

- Perft: all 20 standard positions pass (startpos, Kiwipete, positions 3–6),
  run against the kit-packed `main.py` itself.
- Deterministic: same position in two fresh processes gives the same move
  (startpos b1c3, Kiwipete e2a6, endgame KQ vs KR b4f4 — all match).
- Speed: ~2.4–4.3 s per move (limit: 10 s); slowest ~4.6 s.
- Practice trial 0 vs the house gauntlet (exact packed file, local kit env):
  3 wins, 1 draw, 2 losses (3.5/6) — estimated Elo ~1,364.
  Win vs depth-2 by checkmate; win vs depth-3 by checkmate;
  win vs depth-4 by checkmate; draw vs nodes-100k (adjudicated);
  loss vs nodes-250k by checkmate; loss vs nodes-500k by checkmate.
  Above the 1,154 practice floor; essentially tied with the sunfish
  baseline mean (~1,365), so the prize gate is borderline on one trial.
- v1 (same trial) scored 3.0/6 (~1,279 Elo), so v2 is +0.5 points / +85 Elo.
- Head-to-head vs the sunfish NNUE baseline at 50,000 nodes/move:
  game 1, v2 as White, won by checkmate in 111 plies; game 2 was cut
  short by a VM restart, so the match stands 1–0 unfinished.

## Development notes

Readable v2 source: `v4.py` (7,899 bytes, kept beside the repo but not
committed — the kit's bundle uploads it beside the transcripts). It
packs with `makefun compress v4.py --out main.py` to the installed
`main.py` (3,346 bytes; the payload decompresses byte-for-byte to
`v4.py`). An earlier hand-packed `main.py` (4,089 bytes, own lzma +
base85 stub) was replaced because the transcript check only credits a
packed entrypoint the kit's `makefun compress` wrote. The 4 KB cap
forced trade-offs: futility pruning and the check extension were
dropped to fit null-move pruning, killers, history, and the endgame
king table.
