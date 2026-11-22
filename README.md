# tiny-chess-01 — a 4 KB chess engine

![Python 3.13](https://img.shields.io/badge/python-3.13-blue)
![stdlib only](https://img.shields.io/badge/stdlib-only-green)
![size](https://img.shields.io/badge/main.py-%E2%89%A4%204096%20bytes-orange)
![license](https://img.shields.io/badge/license-MIT-lightgrey)

A chess engine written in Python, packed into a single `main.py` of at most
4,096 bytes, for the make.fun **tiny-chess-01** competition: build the
strongest chess engine in 4 KB of Python.

## How it plays

- 0x88 board, negamax alpha-beta with quiescence search (captures only,
  delta pruning)
- Iterative deepening with a fixed node budget (deterministic: no clock,
  no unseeded randomness), transposition table, killer moves, MVV-LVA
  move ordering, pin-aware legal move generation
- Null-move pruning, check extensions
- Evaluation: material + piece-square tables (middlegame / king endgame),
  passed-pawn bonuses; resigns only when the search is clearly lost
- Talks HTTP to the competition environment (`$MAKEFUN_ENV_URL`):
  `POST /game`, `POST /move`, `POST /resign`

## Layout

- `main.py` — the engine (an lzma payload + unpacker; the payload
  decompresses to the readable source that was developed and tested
  locally, see below)
- `LICENSE` — MIT

## Development

The engine was developed as a readable `main.py` (0x88, perft-verified:
startpos 20/400/8,902 and kiwipete 48/2,039/97,862) and then packed with
lzma + base85. The packed file is byte-identical in behaviour to the
readable source. Local practice-trial Elo is measured with the kit's own
environment and scorer (`program/env/server.mjs` +
`program/scorer/score.mjs`).

## License

MIT — see `LICENSE`.
