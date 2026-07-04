# Maths Question Modelling Tool

A single-file, no-install teaching tool for breaking a word problem down into
maths — the way you'd do it on the board, one step at a time.

**Live demo:** https://amkamz.github.io/math-problem-breakdown-tool/

It walks students through the same three steps for almost any worded problem:

1. **The goal** — fill in *"Find ___ when ___"* in plain words, then watch the
   blanks rewrite themselves into symbols.
2. **Variables** — name each quantity with a letter.
3. **Equations** — turn each fact into an equation, then solve line by line.

Click a highlighted phrase in the problem and the matching piece appears in the
working panel, so the algebra is always tied back to the words that produced it.

## Using it

Open the live demo (or `index.html`) in a browser. The toolbar lives at the
bottom — click **menu** to show it.

- **Present** — the student-facing view. Pick a step, then click the
  highlighted words in the problem.
- **Author** — edit the problem: change the text, mark phrases for each step,
  and write the solution. A live preview mirrors the Present view.
- **Framework** — a one-page explainer of the three-step method.

You can also print a **blank worksheet** or an **answer key** from the toolbar,
and **Export** / **Import** individual problems as JSON.

### On an iPad / phone

Open the live demo in Safari, then **Share → Add to Home Screen** for a
full-screen, app-like icon. Everything in Present mode works. Note that saving
to loose `.json` files is desktop-only (see below); on iPad you get the built-in
problems plus in-browser edits and JSON export.

## Included examples

- **Newton's law of cooling** — find an initial temperature from `T(t) = 15 + 4e^{-3t}`.
- **Andre and Bob's money** — a two-equation "who has how much" puzzle.
- **Rancher's pen** — a fencing optimisation (largest area) problem.

## How problems are stored

Every example is embedded directly in `index.html` (the `PRELOADED` array), so
the app is fully self-contained — the loose `.json` files next to it are
optional. The app picks a storage backend automatically:

- **IndexedDB** (default, works in every browser, including iPad): on first run
  the built-in problems are copied into the browser's local database.
- **File System Access API** (desktop Chrome/Edge only): click **Open folder**
  to read and write real `.json` files in a directory — handy for authoring a
  question bank you keep under version control. This is the only mode that
  touches the loose `.json` files, and it is not available on iOS/iPadOS.

Do your authoring on a desktop with **Open folder**, then present anywhere.

## Problem JSON format

Each problem is one JSON file. Phrases are anchored to the problem text by
**character offsets** (`start` / `end`), not by searching for the words — so a
phrase that appears more than once, or text that later gets edited, never
highlights the wrong spot. `phrase` is kept only as a human-readable label.

```jsonc
{
  "id": "cooling",
  "title": "Newton's law of cooling",
  "text": "The temperature T(t) °C ...",
  "marks": {
    "goal":      [{ "start": 121, "end": 166, "phrase": "...",
                    "findWorded": "the initial temperature", "findSym": "$T(0)$",
                    "whenWorded": "", "whenSym": "" }],
    "variables": [{ "start": 40, "end": 54, "phrase": "...", "content": "Let $t$ = ..." }],
    "equations": [{ "start": 98, "end": 118, "phrase": "...", "content": "$T(t) = ...$" }]
  },
  "solution": ["Set $t = 0$: ...", "..."]
}
```

Wrap maths in `$...$` — it's rendered with KaTeX. Older files that used
`{ "phrase", "occurrence" }` instead of offsets are upgraded automatically the
first time they load.

## Running locally

It's a static file — just open `index.html` in a browser. KaTeX and the fonts
load from a CDN, so the first load needs internet (cached afterwards). To serve
it over HTTP instead:

```bash
npx http-server -p 8000
# then open http://localhost:8000/
```

## License

[MIT](LICENSE)
