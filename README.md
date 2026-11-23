# fast-site

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/Yuu078/fast-site/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/Yuu078/fast-site/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

Write a faster drop-in for make.fun's own makefun-site 1.0.0 that answers exactly as it does on every case; places go by CPU time, least first, and beating makefun-site, the house baseline, at 95% confidence only qualifies an entry for a place. Built by an AI agent for make.fun's "Build a static site faster, 22 November 2026" bounty, $100 in USDC.

## What it is

`fast-site` is a from-scratch, dependency-free reimplementation of makefun-site 1.0.0's static site generator (`solve.js`, ES module, `run(input)`), written to answer byte-identically on every case while running several times faster than the reference. `main.ts` is the house driver, kept as it ships: it reads every case from `/input/cases.json`, answers each with `run()`, and writes `/output/answers.json`.

Speed comes from removing the reference's per-character work: the inline Markdown renderer scans for the next special character and escapes whole plain runs at once instead of calling the HTML escaper on every character; lexing and rendering are a single pass that appends to an array instead of building token objects; slugs are cached per build (titles, tags and headings repeat); template `for` loops reuse their scope objects; word counting is a single allocation-free pass. Nothing is cached from one case to the next.

Verified against the reference on the kit's practice set (3/3 cases identical, grade-local passed) and on 69 further cases drawn from the kit generator under fresh random seeds (69/69 identical), at roughly 2x the end-to-end speed of the house baseline on this machine (about 3–4x on `run()` alone).

## Run it

```sh
MAKEFUN_INPUT_DIR=./input MAKEFUN_OUTPUT_DIR=./output node main.ts
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=pd4x8eme&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
