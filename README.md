# Session Timer

A single-file, self-contained countdown timer for showing remaining time to
students while screen-sharing during online sessions.

## Use it

Open [`timer.html`](timer.html) in any modern browser — no install, no server,
works offline. Click **Fullscreen** for a clean presenting view.

### Features
- Large countdown ring + tabular digits, readable across a shared screen
- Color cues as time runs low (amber under 1 min, red under 30 s), plus a chime at zero
- Presets (5 / 10 / 15 / 25 / 45 / 60 min) or type any number of minutes
- Editable, remembered session label
- `+1 min` to extend on the fly, even while running
- Keyboard: `Space` start/pause · `R` reset · `F` fullscreen · `↑`/`↓` adjust · click ring to hide controls
- Salesforce-themed styling with Codey & Astro–inspired background characters

## Hosting on GitHub Pages

This is a static page, so it can be served directly by GitHub Pages:

1. Push this repo to GitHub.
2. In the repo on GitHub: **Settings → Pages → Build and deployment**,
   set **Source: Deploy from a branch**, branch **main** / **`/ (root)`**, Save.
3. After a minute the site is live at
   `https://<username>.github.io/<repo-name>/timer.html`.

## Notes
- The end-of-timer chime needs one click on the page first (browser autoplay policy).
- The Poppins font loads from Google Fonts when online, falling back to system
  fonts offline.
- The Codey/Astro background figures are original inline-SVG lookalikes, not the
  official Salesforce artwork.
