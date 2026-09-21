# ID Print Bureau

A browser-only tool that crops a scan or PDF of an Indian ID card and produces a
print-ready PDF at exact millimetres.

Unrelated to the thyroid_journal research repo — kept separate deliberately.

## Files

- `index.html` — the entire app. One file, no build step, no dependencies to install.

That filename matters: static hosts (Vercel, Netlify, GitHub Pages) serve
`index.html` at the site root. Any other name gives a 404 on `/`.

## Deploying

Vercel: import the repo, **Framework Preset: Other**, no build command, output
directory left empty. It is a plain static site — nothing to compile.

Opening `index.html` straight off disk works too; `file://` needs no server.

## Sizes it produces

| Document | Size |
|---|---|
| PAN card, Aadhaar PVC, Voter ID (EPIC), driving licence, debit card | 85.60 × 53.98 mm (ISO/IEC 7810 ID-1) |
| Passport photo (India) | 35 × 45 mm |
| Visa photo (2 × 2 in) | 51 × 51 mm |
| Stamp size photo | 20 × 25 mm |
| Custom | any millimetres |

Cards render at 600 dpi and are placed in the PDF at true physical size. In the
print dialog choose **Actual size / 100%** and turn off *Fit to page*, or the
printer shrinks the sheet by a few percent.

## Privacy

Nothing is uploaded. The file is read, cropped and turned into a PDF entirely in
the browser tab. That was a deliberate choice given what these documents are.

## External libraries

Loaded from cdnjs at runtime, not vendored:

- `jspdf 2.5.1` — writes the PDF, loaded eagerly
- `pdf.js 3.11.174` — reads PDF input, loaded lazily on the first PDF dropped

`pdf.worker.min.js` is loaded into the main thread on purpose: a cross-origin
Worker is not allowed, and pdf.js falls back to the main-thread message handler
when `window.pdfjsWorker` is already defined.

## Saving

`saveFile()` has two paths. Inside a Claude artifact viewer the host's own save
prompt takes the blob via `window.claude.use("downloads")`, because that sandbox
makes `<a download>` inert. Everywhere else — Vercel, any static host, `file://` —
it falls back to an ordinary anchor download. Both paths are live; do not remove
either one.

## Claude artifact version

The app is also published as a Claude artifact:
https://claude.ai/artifact/N94YEEyAFMDu8pibXNxuQi

That host wraps the page in its own `<!doctype html>`/`<head>`/`<body>` skeleton,
so it needs a *fragment* rather than a full document. To regenerate it from
`index.html`, drop the wrapper: delete the lines above `<title>`, the
`</head>`/`<body>` pair in the middle, and the closing `</body></html>`. Keep the
`[hidden]{display:none!important}` rule — the JS toggles `el.hidden` throughout,
and several of those elements carry a `display` value from a class that would
otherwise win.
