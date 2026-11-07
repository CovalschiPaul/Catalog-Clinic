# Catalog Clinic

A Paul-Andrei Covalschi portfolio project: a practical product-catalog checker that runs entirely in the browser. Built around catalog analysis, a field I work in, rather than a generic dashboard.

## Run

Open `dist/index.html` in a modern browser. No dependencies, API keys, account or build step. The hosted version serves the same file. Tests require Node.js 18 or newer:

```sh
node tests/check.cjs
```

## What works

CSV import (comma or semicolon), quoted values and multiline cells, column mapping, instant synthetic sample, duplicate SKU checks, missing fields, numeric price/stock validation, self-parent detection, variant collisions, search/severity filters, paginated data preview, issue CSV download, reversible whitespace/deduplication preview, cleaned CSV download, and a share summary containing totals only.

The sample contains 16 rows with 11 findings across 10 rows (38/100 clean-row score). Safe cleanup trims one cell and removes one repeated row, resulting in 15 rows with seven affected rows (53/100). It does not invent missing titles, prices or identifiers.

## Read the code

Everything is in `dist/index.html` to keep the app portable:

1. The `<style>` block defines the cream/forest-green visual language and phone layout.
2. The `engine` script contains pure functions: `parseCSV`, `audit`, `cleanup`, `csv`.
3. The final script connects the buttons, file input, mapping selectors and table to the engine.
4. `tests/check.cjs` extracts the engine and tests it using Node's built-in assertions. No browser test runner is required.

To add a check, add an `add(...)` call in `audit` with a severity, explanation and next step; then add a regression test. To change the style, edit the CSS variables at the beginning of the file. This project was developed with AI assistance; the code is deliberately small enough to inspect and maintain.

## Limits and semantics

- UTF-8 CSV, up to 5 MB, 10,000 data rows, and 100 columns. XLSX is not supported; export from Excel to CSV first.
- SKU matching and variant matching are case-insensitive. Stock is optional; blank stock is permitted. Prices use a decimal dot and no currency symbols.
- Record numbers count the header and skip fully empty lines; multiline quoted values are one record.
- Score is the percentage of rows with zero findings, rounded to an integer. It is not a business KPI, accuracy guarantee or marketplace certification.
- Cleanup trims values, then removes identical rows across all columns. It retains conflicting records. Restore original reverses the preview.
- The data is held in memory, with no fetch calls, storage or analytics. Refresh clears it. Hosting providers may still log ordinary page requests; they do not receive CSV contents from this code.
- Export prefixes formula-like text with an apostrophe for spreadsheet safety. This intentionally changes such text in exported files. Import SKU columns as text in Excel to preserve leading zeros; CSV itself has no cell types.
- No user testimonials, visitor counts, revenue claims or fabricated results are included.

## Sharing the project

The app includes a sample that works with one click, helpful search metadata and a totals-only share card. Share the actual demo with a specific question (for example, “Would this catch a problem in your product feed?”), and improve it using real feedback. These features make it easier to demonstrate, but do not guarantee views.
