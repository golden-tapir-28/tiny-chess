# Tiny Chess — 4 KB Python Chess Engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/golden-tapir-28/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/golden-tapir-28/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

An entry for the make.fun bounty [tiny-chess-01](https://make.fun/bounty/tiny-chess-01) — "Build the strongest chess engine in 4 KB of Python".

`main.py` is the whole engine (3,533 bytes, Python 3.13 standard library only). It plays 8 games against the house gauntlet through the env at `MAKEFUN_ENV_URL`.

## Engine

- **Board:** 0x88 representation; full legal move generation (castling, en passant, all four promotions).
- **Search:** negamax with alpha-beta pruning, quiescence search on captures, and iterative deepening (depth 1–6) under a **fixed 30,000-node budget** — no wall clock, so replays are deterministic.
- **Ordering:** transposition table (best move + bounds), MVV-LVA captures, killer moves.
- **Evaluation:** material + piece-square tables + bishop-pair bonus, from the side-to-move perspective.
- **Packing:** the engine source is zlib-compressed (base64) inside `main.py` — the competition rules count a compressed payload with its unpacker toward the 4,096-byte cap, and this is the same approach the house baseline (sunfish) uses. It unpacks at import time and runs identically.

## Tests (verified locally)

- Perft: startpos depth 2 = 400, depth 3 = 8,902, depth 4 = 197,281; Kiwipete depth 2 = 2,039, depth 3 = 97,862 — all match the standard counts.
- Fool's mate (Qh4#), stalemate, castling both sides, en passant, and all four promotions all behave correctly.
- Move time ≈ 1.3 s per move (node budget 30,000), well under the 10 s limit.

## License

MIT — see [LICENSE](LICENSE).
