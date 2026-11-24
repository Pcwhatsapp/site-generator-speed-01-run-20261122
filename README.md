[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/Pcwhatsapp/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/Pcwhatsapp/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

# site-generator-speed

A faster drop-in for makefun-site 1.0.0: `solve.js` answers exactly as makefun-site does on every case, faster.

## Layout

- `solve.js` — the whole generator, dependency-free. `run(input)` returns the output tree for one case's input.
- `main.ts` — the driver the harness runs: reads `/input/cases.json`, answers each case with `run`, writes `/output/answers.json`.

## How it is faster

The reference semantics are ported exactly; only the hot paths changed:

- Markdown lexing classifies lines by char codes instead of seven regexes per line.
- Markdown inline rendering escapes plain runs in bulk and finds `**`, `*`, `]` closers with `indexOf` plus a backslash-run parity check (equivalent to the reference's left-to-right escape scan at every call site).
- Front matter finds its close line with `indexOf`, so a source's body is never split and re-joined; only the small front-matter section is split.
- `slugify`/`longDate` are memoised per `run()` (pure functions; the memo is cleared at the start of every run, so nothing carries between cases).
- Template rendering walks into one shared string array per page instead of building a fresh string per nested render.

## Licence

MIT.
