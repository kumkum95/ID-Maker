# ID Print Bureau

A browser-only tool that crops a scan or PDF of an Indian ID card and produces a
print-ready PDF at exact millimetres.

**Live page:** https://claude.ai/artifact/N94YEEyAFMDu8pibXNxuQi

Unrelated to the thyroid_journal research repo — kept in its own folder deliberately.

## Files

- `id-print-bureau.html` — the whole app. One file, no build step, no dependencies to install.

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

## Editing it

The HTML is a fragment — no `<!doctype>`, `<html>`, `<head>` or `<body>` tags.
The Claude artifact host wraps it at publish time. Opening the file directly in a
browser still renders and the cropping works, but the two Save buttons will report
that saving is unavailable, because they go through the host's save prompt
(`window.claude.use("downloads")`), which only exists on claude.ai.

To publish changes to the same URL, republish this file path with the artifact URL
passed as `url` — publishing without it creates a second, separate artifact.
