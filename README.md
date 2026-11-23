# site-generator

[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/kornztt/site-generator/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/kornztt/site-generator/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

A faster drop-in replacement for makefun-site 1.0.0, built for the
[site-generator-speed-01-run-20261122](https://make.fun/bounty/site-generator-speed-01-run-20261122)
bounty. `solve.js` exports `run(input)`, which returns exactly the output tree
makefun-site 1.0.0's `build` returns for the same input — every page's
`index.html`, the index and tag pages, `build-report.txt` and every static file,
byte for byte. `main.ts` is the harness driver: it reads every case from
`/input/cases.json`, answers each with `run`, and writes `/output/answers.json`.

Speed, without changing any result:

- Templates are compiled once to JavaScript functions instead of being walked
  as a node tree on every render.
- Markdown renders in one pass: a native scan jumps straight to the next
  special character, literal runs are sliced and escaped in bulk — no token
  objects and no per-character work.
- Slug results are memoised, since titles and tag names repeat constantly.
- Words are counted with a plain character scan, not a regex, and each source's
  card is built once (a page reuses its card instead of building it twice).
- Front matter is scanned with `indexOf`, and the body stays a slice of the
  source instead of being split apart and joined back together.

Verified on the kit's practice instance: 3 of 3 cases identical to the
reference (practice score 1000), plus 120 differential-fuzz cases (mutated
sites, edge-case front matter, links, CRLF, duplicate slugs, missing
templates) all byte-identical to the reference build.

## Layout

- `main.ts` — the driver (unchanged from the kit's house entry)
- `solve.js` — the fast implementation, dependency-free
- `package.json` — `{"type": "module"}`
- `LICENSE` — MIT
