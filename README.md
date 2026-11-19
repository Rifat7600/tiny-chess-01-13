![make.fun](https://make.fun/badge/tiny-chess-01.svg)
![rank](https://make.fun/badge/tiny-chess-01/Rifat7600/tiny-chess-01-13/rank.svg)
![reward](https://make.fun/badge/tiny-chess-01/Rifat7600/tiny-chess-01-13/reward.svg)

# Tiny Chess 4K — Rich Eval

A chess engine in 4 KB of Python for the make.fun tiny-chess-01 bounty:
a single `main.py` (4,072 bytes as committed), Python 3.13 standard library only.

## The engine

- 0x88 board, alpha-beta search with quiescence, a transposition table,
  killer moves and null-move pruning, on a fixed budget of 45,000 nodes
  per move — no clock, so every trial replays byte-identically.
- The eval is the differentiator: material (100/320/330/500/900) plus
  piece-square terms, the bishop pair, knight and bishop mobility, passed
  pawns, rooks on open and semi-open files, and a king pawn shield.
  The king piece-square table is zeroed (a size trade-off; the shield
  term covers middlegame king safety).
- Every eval term is cheap arithmetic over precomputed tables, so node
  throughput stays high enough to keep depth 3–4 in the middlegame.
  Killer moves halved the depth-4 node cost (about 13k vs 29k nodes),
  which is what makes the search strong at this budget.

## Trial result (practice trial-0, 6 games)

**Elo 1451 — 4 wins, 0 draws, 2 losses.** Beats depth-2, depth-3,
nodes-100k and nodes-250k (all by checkmate); loses to depth-4 and
nodes-500k (both by checkmate). This ties the kept 1451 entry rather
than beating it: the rich eval plus killers fixed the walk-into-mate
problem against the nodes engines, but depth-4 still finds a quick
mate (50 plies), so the engine is not yet clearly stronger overall.

## Files

- `main.py` — the packed entry (compress the source with the kit).
- `source.py` — the readable source (not committed to the entry snapshot).
- `LICENSE` — MIT.

## Build

```sh
node <kit>/cli/makefun.mjs compress source.py --out main.py
```
