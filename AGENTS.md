# AGENTS.md

Contract for any agent (or human) changing this repository. Read it fully before editing.

## Purpose

This repository is a static GitHub Pages site made of small, single-file HTML tools.
Each tool runs entirely in the browser. There is no backend, no build step, and no
package manager. `index.html` is a searchable gallery generated at runtime from
`catalog.json`.

## Layout

```
/
  LICENSE          # MIT - never modify
  README.md        # short human-facing overview
  AGENTS.md        # this contract
  catalog.json     # tool catalog (source of truth for the gallery)
  index.html       # searchable gallery, reads catalog.json
  tools/
    <id>.html      # exactly one self-contained file per tool
```

Rules:

- Every tool is exactly one file: `tools/<id>.html`. No shared CSS/JS files, no
  sub-folders, no assets. Inline `<style>` and `<script>` only.
- `<id>` is kebab-case (`^[a-z0-9]+(-[a-z0-9]+)*$`) and equals the filename without `.html`.
- Every file in `tools/` has exactly one entry in `catalog.json`, and every entry points
  to an existing file.
- All links are relative (`tools/<id>.html`, `../index.html`). Never use root-absolute
  paths (`/tools/...`) or a hard-coded repository prefix; they break `file://` and
  project Pages URLs.

## Stack

- Vanilla HTML, CSS, and JavaScript only.
- No frameworks, bundlers, transpilers, `package.json`, npm, or CDN libraries.
- No external fonts, scripts, stylesheets, images, or network calls from tools.
  (`index.html` fetches only the local, relative `catalog.json`.)

## `catalog.json` shape

A single object with a `tools` array. Every entry has exactly these fields:

```json
{
  "tools": [
    {
      "id": "example-tool",
      "title": "Example Tool",
      "summary": "One short sentence describing what it does.",
      "tags": ["tag-one", "tag-two"],
      "path": "tools/example-tool.html"
    }
  ]
}
```

| Field     | Type     | Rule                                                        |
|-----------|----------|-------------------------------------------------------------|
| `id`      | string   | kebab-case, unique, equals filename without `.html`         |
| `title`   | string   | short, human-readable                                       |
| `summary` | string   | one short sentence, ends with a period                      |
| `tags`    | string[] | lowercase, kebab-case, 2-6 items                            |
| `path`    | string   | `tools/<id>.html`, relative, no leading `/`                 |

Order in the array is the display order in the gallery.

### Embedded fallback copy

Browsers such as Chrome block `fetch()` for `file://` URLs. So the gallery still works
when opened directly from disk, `index.html` contains an embedded copy of the catalog in
`<script type="application/json" id="catalog-fallback">`. It is used only when fetching
`catalog.json` fails, and a soft notice is shown when that happens.

The embedded copy must be identical in content to `catalog.json`. When served over HTTP,
`index.html` compares the two and logs a `console.warn` if they differ.

## Adding a tool (checklist)

1. Pick an `id` (kebab-case) that is not already in `catalog.json`.
2. Create `tools/<id>.html` as a single self-contained file that meets the tool
   requirements below. Start from an existing tool to keep the look consistent.
3. Add an entry to the `tools` array in `catalog.json` with `id`, `title`, `summary`,
   `tags`, and `path: "tools/<id>.html"`.
4. Paste the same entry into the embedded fallback block in `index.html`.
5. Open `index.html`, search for the new tool by title, by a word from the summary, and
   by a tag, and follow the card link to confirm it opens.
6. Run the acceptance checks at the end of this file.

## Tool requirements

Every tool page must have:

- `<!doctype html>`, `<html lang="en">`, `<meta charset="utf-8">`,
  `<meta name="viewport" content="width=device-width, initial-scale=1">`, a descriptive
  `<title>`, and a `<meta name="description">`.
- A back link to `../index.html`, an `<h1>` title, and a one-line description.
- Every input has a visible `<label for="...">` and a sensible default value, so the
  page shows a useful result immediately on load.
- Live results: outputs recompute on every `input`/`change` event. No submit button is
  required; an optional Reset button restores defaults.
- Soft validation: empty, non-numeric, negative, zero, or out-of-range values produce
  an inline message next to the field (`aria-invalid="true"` plus a text message) and
  the results area explains why nothing can be computed. Never throw uncaught errors,
  never show `NaN`/`Infinity`, never use `alert()`, `confirm()`, or `prompt()`.
- Any formula or assumption behind a result is stated in a short note on the page.
- Self-contained, responsive styling that is readable at 360 px width and supports
  light and dark color schemes.

## Content constraints

- English only, in UI text, code comments, and docs.
- No secrets, API keys, tokens, or credentials.
- No personal data: no names of real people, emails, phone numbers, addresses, wallet
  addresses, or account numbers - not in defaults, placeholders, examples, or copy.
- Do not name third-party creators, and do not add "inspired by" or similar
  attribution lines.
- Do not mention any person, lab, or external example gallery in committed text.
- Placeholder data (rates, prices) must be clearly labeled as editable placeholders,
  never presented as live or authoritative.

## Acceptance checks (verify before finishing)

- [ ] `catalog.json` is valid JSON with the shape above; every `id` matches its `path`
      and every `path` exists under `tools/`.
- [ ] Every file in `tools/` is listed in `catalog.json`.
- [ ] The embedded fallback in `index.html` matches `catalog.json`.
- [ ] `index.html` search filters case-insensitively by title, summary, and tags; the
      empty state appears when nothing matches; a soft notice appears if `catalog.json`
      cannot be loaded.
- [ ] Each tool has labels, defaults, live output, and soft inline errors (try empty,
      negative, zero, and non-numeric input), and logs no console errors.
- [ ] Every page works when opened via `file://` and when served from a static server
      under a sub-path (as on GitHub Pages project sites), using relative paths only.
- [ ] No build step was introduced; no external network requests.
- [ ] English only; no PII or secrets; no attribution lines; `LICENSE` unchanged.
