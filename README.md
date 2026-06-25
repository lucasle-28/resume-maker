# Resume Bank

A static, single-file web app that assembles a tailored, print-ready resume from a master bank you keep in one Google Sheet. Pick a target (FPGA / ASIC, Embedded, DSP, …) and it auto-builds the resume; tweak, then export to PDF, .docx, or copy. **All content lives in the Sheet — you never edit the code.**

## Files
- `index.html` — the whole app (no build step, no embedded data)
- `bank-seed.csv` — import once into a fresh Google Sheet to get the columns + your current content
- `README.md` — this file

## Host on GitHub Pages
1. New repo, add `index.html` to the root.
2. Settings → Pages → Deploy from a branch → `main` / root.
3. Open `https://<you>.github.io/<repo>/`.

## Set up the Sheet (one time, no code)
1. New Google Sheet, `File → Import → Upload` the `bank-seed.csv` (replace current sheet).
2. `File → Share → Publish to web` → the tab → **Comma-separated values (.csv)** → Publish.
3. In the app: **Source** → paste the link → **Connect**. Or bookmark `…/?sheet=<PUBLISHED-CSV-URL>`.

Edit content in the Sheet, hit **Refresh** in the app.

## How the Sheet works
One tab. Columns: `Type, Title, Tags, Presets, Date, Bullet 1, Bullet 2, Bullet 3, Bullet 4, Last Modified, Notes`

- **Type** — one of:
  - `PROFILE` — permanent header/education/skills. Hidden from the picker, always on the resume.
    - `Name` → Bullet 1 = your name
    - `Contact` → Bullet 1 = your contact line
    - `Education` → Bullet 1 = school, Bullet 2 = degree line (GPA inline), Date = grad date
    - `Skill` → one row per line (Bullet 1 = `Languages: …`); add as many as you like
  - `Coursework` — one row per preset; Presets = which preset, Bullet 1 = comma-separated courses
  - `Experience` / `Project` / `Leadership` — your entries
- **Title** — prints as the bold entry heading exactly as written
- **Tags** — free labels for search (comma-separated; use the Sheets multi-select dropdown)
- **Presets** — which preset buttons this row belongs to (comma-separated). Preset names may contain `/` (e.g. `FPGA / ASIC`); rows are separated only by commas.
- **Date** — right-aligned date on the heading
- **Bullet 1–4** — blank cells are skipped (2–4 bullets typical)
- **Last Modified** — shown as a freshness note in the picker; never printed
- **Notes** — picker-only reminders; never printed

## Using it
- Click a **preset** → filters to its entries and auto-selects them (plus that preset's coursework). **All** shows everything and keeps your selection, so you can add an off-preset entry.
- **Coursework** shows as one line; **More options** expands individual courses to add/drop.
- Tick/untick entries → the **live preview** on the right updates instantly.
- **Export** (top right): Save as PDF · Download .docx · Copy rich text (for Google Docs/Word) · Copy as Markdown · Copy as text · Copy share link. PDF and .docx use the active preset in the filename.

## Notes
- No Sheet connected → a connect prompt, not a blank page. A failed pull shows an error state (no stale fallback).
- `.docx` export pulls a small library from a CDN the first time you use it; everything else works offline once loaded.
- Selections persist in your browser when hosted/local. Some embedded previews block storage; it still runs, just without persistence.
