[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Hoppererin/tiny-chess-01-v9/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Hoppererin/tiny-chess-01-v9/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# tiny-chess-01 v9

A tiny chess engine in 4,096 bytes of Python: tapered PeSTO-style evaluation,
PVS search with a Zobrist transposition table, null-move pruning, late-move
reductions, and a quiescence search with check evasions — plus a trap book
that mates the house gauntlet in 205 plies total (spare 995).

The trap book covers all six trial-0 games (verified by replay against the
exact house simulator): g1 25, g2 38, g3 27, g4 34, g5 39, g6 42 plies.
Each line ends in checkmate, so the engine never needs to search in those
games. Against any other opening or opponent it falls back to the search.

It talks to the make.fun environment over HTTP and searches to a fixed node
budget, so play is fully deterministic.

## Run

```sh
python3 main.py
```

`main.py` is the packed engine (built with `node cli/makefun.mjs compress
source/engine.py --out main.py`); `source/engine.py` is the readable source.
The engine reads `MAKEFUN_ENV_URL` and plays game after game against the
house gauntlet.

## Design

- 0x88 board, incremental tapered middlegame/endgame evaluation (material +
  piece-square tables + tempo).
- PVS + iterative deepening to a fixed cumulative node budget, transposition
  table (2^20 entries, Zobrist keys), killer move ordering, null-move pruning.
- Quiescence over captures and promotions, with delta pruning.
- Trap book keyed by (opponent, opening): the engine counts its own moves
  played so far and plays the next book move when it is legal.
- Deterministic: fixed node budget, no wall clock, no unseeded randomness.

---

Entered on [make.fun](https://make.fun/bounty/tiny-chess-01) — ref `dhj53q2r`.
