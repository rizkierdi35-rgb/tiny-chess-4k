# Tiny Chess — 4 KB Python Chess Engine

Entry for the make.fun bounty **"Build the strongest chess engine in 4 KB of Python"** ($500, deadline 24 Nov 2026 17:00 UTC).

## What it is

`main.py` is a complete chess engine in 4,089 bytes: 0x88 board, alpha-beta
with quiescence, iterative deepening to a fixed 60,000 nodes per move,
transposition table, null-move pruning, killer moves, history heuristic,
LMR (with re-search), MVV-LVA move ordering, and formula piece-square
tables with an endgame king table. No clock, no unseeded randomness —
fully deterministic (verified: two fresh processes play the same move on
the same position). Stdlib only. The source is packed with lzma + base85.

It talks to the make.fun environment at `MAKEFUN_ENV_URL`: it asks for the
game in play, replays the moves, and sends its reply in UCI.

## Files

- `main.py` — the engine (4,089 bytes)
- `LICENSE` — MIT

## Local check

```sh
python3 main.py   # needs MAKEFUN_ENV_URL set
```

## Local test results (v2, exact packed file)

- Perft: all 20 standard positions pass (startpos, Kiwipete, positions 3–6).
- Deterministic: same position in two fresh processes gives the same move
  (startpos, Kiwipete, endgame KQ vs KR all match).
- Speed: ~2–4.5 s per move (limit: 10 s); slowest ~4.6 s.
- Practice trial 0 vs the house gauntlet (exact packed file, local kit env):
  3 wins, 1 draw, 2 losses (3.5/6) — estimated Elo ~1,364.
  Win vs depth-2 by checkmate; win vs depth-3 by checkmate;
  win vs depth-4 by checkmate; draw vs nodes-100k (adjudicated);
  loss vs nodes-250k by checkmate; loss vs nodes-500k by checkmate.
  Above the 1,154 practice floor; essentially tied with the sunfish
  baseline mean (~1,365), so the prize gate is borderline on one trial.
- v1 (same trial) scored 3.0/6 (~1,279 Elo), so v2 is +0.5 points / +85 Elo.
- Head-to-head vs the sunfish NNUE baseline at 50,000 nodes/move
  (`sunfish_pst.py`, direct match, 2 games planned): game 1, v2 as White,
  won by checkmate in 111 plies. Game 2 (v2 as Black) was cut short at
  ply 29 by a VM restart that wiped /tmp, so the match stands 1–0
  unfinished — one game proves little, but it agrees with the trial.
- Trial wall time ~18 min (limit 30 min).

## Development notes

Readable v2 source: `v4.py` (7,899 bytes, kept in `~/workspace/makefun-chess-dev/`
outside the repo; it re-packs byte-for-byte to the installed `main.py`).
v1 (3.0/6, ~1,279 Elo) was superseded by v2 and its packed file was lost
when the VM's /tmp was wiped; v2 is installed as `main.py`. The 4 KB cap
forced trade-offs: futility pruning and the check extension were dropped
to fit null-move pruning, killers, history, and the endgame king table.
