# Pocket Knight

A chess engine in 4 KB of Python, built for the make.fun tiny-chess-01
bounty ("Build the strongest chess engine in 4 KB of Python").

## How it plays

- 0x88 board, fully legal move generation (castling, en passant, promotion,
  fifty-move and repetition rules), verified against perft on the standard
  test positions.
- Alpha-beta search with iterative deepening, a transposition table,
  killer moves, MVV-LVA + promotion move ordering, and quiescence search.
- Evaluation: material, piece-square tables (with a tapered king table that
  shifts from middlegame to endgame), bishop-pair bonus, and a tempo bonus.
- Deterministic: a fixed 100,000-node budget per move, never the clock, so
  the same position always produces the same move.

## Files

- `main.py` — the entry point (3,822 bytes). It is the `makefun compress`
  packing of `source.py`: an LZMA stream of the readable source, unpacked
  and executed with the standard library only.
- `source.py` — the readable engine source (about 8.9 KB). It is kept in
  this folder, uncommitted, so the build can be reproduced and reviewed;
  `makefun compress source.py --out main.py` regenerates the entry point.
- `LICENSE` — MIT.
- `.makefun/owner.json` — the wallet that receives any prize.

## Local strength (practice gauntlet, kit scorer)

Practice trial-0, 6 games against the kit's house engine:
3 won, 2 drawn, 1 lost — **Elo 1451** (beats the depth-4 anchor at 1199
and the nodes-100k anchor at 1392; draws the nodes-500k anchor at 1596).

Practice trial-1 (different openings): 3 won, 1 drawn, 2 lost —
**Elo 1364**. Two-trial average about 1407.

## Run it

The engine speaks the competition protocol: it reads `MAKEFUN_ENV_URL`,
plays a game with `POST /game` and `POST /move`, and prints the result
to stderr. It needs nothing but Python 3.13's standard library.
