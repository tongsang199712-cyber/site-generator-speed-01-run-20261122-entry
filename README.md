# site-generator-speed-01-entry

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/tongsang199712-cyber/site-generator-speed-01-run-20261122-entry/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/tongsang199712-cyber/site-generator-speed-01-run-20261122-entry/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A fast drop-in for makefun-site 1.0.0: `solve.js` answers exactly as makefun-site does for every site, and `main.ts` is the driver the harness runs (`node main.ts`, reading `/input/cases.json`, writing `/output/answers.json`).

## What it does

`solve.js` reimplements makefun-site's whole build — front matter, permalinks, its Markdown subset, collections, tags and its template dialect — in one dependency-free ES module, tuned for speed:

- Markdown renders in a single pass over the lines (no token list), and inline rendering jumps from one markup character to the next with `indexOf`, escaping plain stretches in bulk instead of one character at a time.
- Templates are compiled to JavaScript functions (`new Function`) with the same scope-chain semantics as the reference interpreter, so rendering is native control flow instead of node dispatch.
- `slugify` memoises per run and skips NFKD/mark-stripping for ASCII text; word counts and line trimming are done with direct character tests; the front-matter line array is reused by the Markdown lexer instead of being rejoined and re-split.
- No state is kept between `run()` calls: every cache is created fresh inside `run()`, so each case is answered alone. No clock, environment, randomness or I/O is read.

On the kit's practice set it matches makefun-site byte for byte on all 3 cases and runs about 2× faster than the house baseline (measured end to end, ~0.58 s vs ~0.99 s per run of `main.ts` on this machine).

## Run it

```sh
node main.ts   # with MAKEFUN_INPUT_DIR / MAKEFUN_OUTPUT_DIR set outside a container
```

## The files

- `solve.js`: the entry. `run(input)` returns makefun-site's answer for one case's input.
- `main.ts`: the driver (the house baseline's, unchanged).
- `package.json`: `{"type": "module"}`.
- `LICENSE`: MIT.

<sub>Entered on [make.fun](https://make.fun/?ref=dd7e8ra1&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
