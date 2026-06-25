# Resume Bank

A static, single-file web app that turns your master line-item bank into a filterable picker: tag by role flavor, tick the bullets you want, and export an assembled resume. **All data lives in a Google Sheet** — there is no data in the code. Edit the Sheet, the site re-pulls. You never edit `index.html`.

## Files
- `index.html` — the whole app (no build step, no dependencies, no embedded data)
- `bank-seed.csv` — import this once into a fresh Google Sheet to start with the right columns + your current bullets
- `README.md` — this file

## Host it on GitHub Pages
1. Create a repo (e.g. `resume-bank`) and add `index.html` to the root.
2. Repo **Settings → Pages → Source: Deploy from a branch**, branch `main`, folder `/root`.
3. Open `https://<you>.github.io/resume-bank/`.

## Connect your Google Sheet (one time, no code)
1. Make a Sheet and `File → Import` the `bank-seed.csv` so the columns line up.
   - Columns: **Section, Entry, Bullet, Tags, Number, Source, Best, Notes**
   - `Tags` = comma/slash/pipe separated (e.g. `ARCH, HLS, RTL`). Recognized: RTL, HLS, ARCH, ML, EMB, SW, LEAD, EDU, TITLE (others still work, just uncolored).
   - `Best` = `TRUE` / `x` / `yes` marks the recommended variant (shows ★).
2. `File → Share → Publish to web`, select the tab, format **Comma-separated values (.csv)**, **Publish**.
3. Open the app, click **Source**, paste that link, **Connect**. (A normal `/edit` share link also works if the sheet is "anyone with the link can view".)

Two ways to set the source, both code-free:
- **Source button** — paste once; the link is remembered in your browser.
- **URL parameter** — visit `https://<you>.github.io/resume-bank/?sheet=<PUBLISHED-CSV-URL>` and bookmark it. Handy for a new device or sharing the exact view.

From then on: edit bullets in the Sheet → click **Refresh**.

## Using it
- **Presets / Section / Tag** filter the list; **Recommended only** shows just the ★ variants.
- Tick bullets to drop them into **Build**; the meter is a rough one-page estimate.
- **Copy Markdown**, **Copy text**, or **Download .md** to move the draft into your resume.

## Behavior notes
- No Sheet connected → the page shows a "Connect your Google Sheet" prompt, not a blank screen.
- If a pull fails (sharing off, offline), it shows a clear error state and a Try again button — it does not fall back to stale data.
- Selections persist in your browser when hosted or opened locally. Some embedded previews block browser storage; it just runs without persistence there.
