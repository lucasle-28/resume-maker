# Resume Bank

A static, single-file web app that assembles a tailored, print-ready resume from a master bank you keep in one Google Sheet. Pick a target (FPGA / ASIC, Embedded, DSP, …) and it auto-builds the resume; tweak, then export to PDF, .docx, or copy. **All content lives in the Sheet — you never edit the code.**

## Files
- `index.html` — the whole app (no build step, no embedded data)
- `bank-seed.csv` — import once into a fresh Google Sheet to get the layout + your current content
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

## How the Sheet is read (the layout)
One tab, read top-to-bottom. Sections come from **divider rows**, not a column — so the sheet reads like an outline.

**Divider row** — column A is exactly `PROFILE`, `COURSEWORK`, `EXPERIENCE`, `PROJECTS`, or `LEADERSHIP` and the rest of the row is empty. Everything below it belongs to that section until the next divider.

**The row right under each divider is skipped** — use it for a decorative column label (e.g. `Title | Tags | Presets | Date | Bullet 1 …`). It's for your eyes only.
> ⚠️ The skip is unconditional: always keep one row (a label or a blank) directly under every divider before your real data. The first real row under a bare divider would be eaten.

**Blank rows are ignored** (spaces-only too), so you can space sections out freely.

**Columns are fixed by position** (no header needed):

| A | B | C | D | E | F | G | H | I | J |
|---|---|---|---|---|---|---|---|---|---|
| Title | Tags | Presets | Date | Bullet 1 | Bullet 2 | Bullet 3 | Bullet 4 | Last Modified | Notes |

- **Title** — prints as the bold entry heading exactly as written.
- **Tags** — free labels for search (comma-separated; Sheets multi-select dropdown works).
- **Presets** — which preset buttons this row belongs to (comma-separated). Preset names may contain `/` (e.g. `FPGA / ASIC`); rows are separated only by commas.
- **Date** — right-aligned date on the heading.
- **Bullet 1–4** — blank cells skipped (2–4 bullets typical).
- **Last Modified** — freshness note in the picker; never printed.
- **Notes** — picker-only; never printed.

**PROFILE section** (hidden from picker, always on the resume) — slim, values packed left-to-right after column A:
- `Name` → next cell = your name
- `Contact` → one item per cell across the row (`Contact | U.S. Citizen | email | phone | linkedin | github`); joined with " • " on the resume. Add/remove a contact item = add/clear a cell.
- `Education` → next cells = school, then degree line (GPA inline), then grad date

**SKILLS section** — one row per preset, fixed category columns: `Preset | Languages | Tools | Hardware`. Each category cell is a comma-separated list. Click a preset → its skills row renders as the three split lines; **All** merges and de-dupes every preset's skills.

**COURSEWORK section** — one row per course: Title = the course name, Presets = which presets it belongs to. The app collects the courses tagged to the active preset into the single "Relevant Coursework" line; More options lets you add/drop individual courses. (All shows the deduped union of every course.)

## Using it
- Click a **preset** → filters to its entries and auto-selects them (plus that preset's coursework). **All** shows everything and keeps your selection, so you can add an off-preset entry.
- **Coursework** shows as one line; **More options** expands the individual courses to add/drop.
- Tick/untick entries → the **live preview** updates instantly.
- **Export** (top right): Save as PDF · Download .docx · Copy rich text (Docs/Word) · Copy as Markdown · Copy as text · Copy share link. PDF/.docx filenames use the active preset.

## Notes
- No Sheet connected → a connect prompt, not a blank page. A failed pull shows an error state (no stale fallback).
- `.docx` export pulls a small library from a CDN the first time; everything else works offline once loaded.
- Selections persist in your browser when hosted/local. Some embedded previews block storage; it still runs, just without persistence.
- Don't sort the tab — section membership depends on row position relative to the dividers.
