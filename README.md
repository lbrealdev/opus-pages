# opus-pages

A static GitHub Pages site of small, single-file HTML tools. Each tool is one self-contained
`.html` file that runs entirely in the browser: no build step, no dependencies, no network calls.
`index.html` is a searchable gallery built from `catalog.json`.

## Tools

| Tool | What it does |
|------|--------------|
| [Credit Calculator](tools/credit-calculator.html) | Monthly payment, total interest, and fee impact of an installment loan |
| [FX Quick](tools/fx-quick.html) | EUR / USD / BRL conversion with rates you edit yourself |
| [Unit Cost: API vs Homelab](tools/unit-cost.html) | Cost of one job on a token-priced API vs your own GPU |
| [Cron Explain](tools/cron-explain.html) | Cron expressions to plain English, and common phrases back to cron |
| [Transfer ETA](tools/transfer-eta.html) | Transfer time from data size and effective throughput |

## Use it

- **Locally, no server:** open `index.html` (or any file in `tools/`) directly in a browser.
  Some browsers block reading `catalog.json` from `file://`; the gallery then falls back to a
  built-in copy of the catalog and shows a short notice.
- **Locally, with any static server:** for example `python3 -m http.server` in the repository
  root, then visit `http://localhost:8000/`.
- **GitHub Pages:** in the repository settings, enable Pages with "Deploy from a branch",
  branch `main`, folder `/ (root)`. All links are relative, so the site works under the
  project sub-path.

## Add a tool

Follow the checklist in [`AGENTS.md`](AGENTS.md): create `tools/<id>.html`, add an entry to
`catalog.json` (and its embedded copy in `index.html`), then verify it from `index.html`.

## License

[MIT](LICENSE)
