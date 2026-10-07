# kifu — can a model teach itself Go?

**The page: https://w-is-h.github.io/kifu-bench/**

Four players play 9×9 Go against KataGo's human-rank profiles (20k to 9d), each picking its own opponent before every game: Opus 5.5 in Claude Code and GPT-6.1 Sol in Codex, at medium and at high reasoning effort. Every game is a new session: the conversations of the earlier ones are not carried over; the player's memory is its working directory, and what it knows of its earlier games is what it left there. It may write and run helper code — Opus through a shell, Sol as the JavaScript Codex runs for it — to inspect the position and keep records, and it is told not to build an engine or search over moves; an audit reads every call it made. If the player learns from its files, its Elo climbs. 200 games each, played 5–7 October 2026.

## What happened

- Nobody got better. Opus medium won 14 of its first 50 games and 13 of its last 50, against the same 12k–14k opponents; Opus high 15 then 13; Sol medium 4 then 6 and Sol high 10 then 8, nearly always against 20k, the weakest profile. KataGo's points lost per game, the measure of a player's own mistakes, stayed flat: 3.8 → 3.3, 3.7 → 4.2, 5.1 → 5.0, 4.3 → 5.0.
- The notes grew without converging. Opus's CLAUDE.md went from 35 lines to 3,820 (medium) and 2,306 (high); Sol's AGENTS.md from 13 to 411 and 26 to 362. Each game adds its lessons beside the old ones, all of equal weight; nothing is ranked or retired, and the blunders per game did not fall (4.9 → 4.8, 5.1 → 6.3, 7.3 → 8.0, 7.0 → 8.2).
- The helpers did not help. Opus wrote a liberty counter and a small board library in its first games and ran them in every game after (2.6 shell calls a game at the start, 9.4 at the end). Sol kept per-move records instead, 2,000–2,800 files by the end, and one reusable script, through 35–49 code cells a game. Neither turned tooling into results.
- Nobody cheated: the audit found no engine and no move search in any game.

## What is here

The page as built, and its data as plain files; serve the folder with any static server to run it (`python3 -m http.server`).

- `data/run.json` — the players, the ladder (20k = 0 … 9d = 1221), every game's summary and the Elo after each.
- `data/<player>/<n>/game.json` — the moves, KataGo's analysis, the reviewer's write-up and the audit.
- `data/<player>/<n>/session.json` — the session's transcript: messages, reasoning, tool calls and their results.
- `data/<player>/<n>/files.json` — the player's files after the game: the ten it used in the most games, every version's text in `data/blobs/`; the rest are counted.

The harness, the Go server and the page's source live in a private repository; this is the result.
