# Wedding Timeline — notes

A single-file app in `index.html`, with no build step and no dependencies. Live at
https://candlebird.github.io/wedding-timeline/ (every push to `main` redeploys it to GitHub Pages).

## Running locally

`python -m http.server 8765 --bind 127.0.0.1`, then open http://127.0.0.1:8765/index.html.
Port 8765 on 127.0.0.1/localhost is in the wedding API's CORS list, so online saving works locally too.

## Data

- The timeline autosaves to localStorage (`weddingTimeline.data`). **⋯ → Download a backup** and **Open a backup** move it as a `.json` file.
- **Share → Save online** stores it in the wedding API (the sibling `WeddingApi` Cloudflare Worker, kind `timeline`).
  You then get two links:
  - **Edit link** `…/#edit=<id>.<key>`: opens the timeline for editing on any device. The key is only in this link and in localStorage (`weddingTimeline.cloud`).
  - **View link** `…/#view=<id>`, optionally `&role=<name>`: read-only, opens filtered to that role. Give it to vendors, family and the wedding party.
- Changes save online about 1.5 seconds after you stop typing. Other devices pick up changes every 20 seconds and when the tab comes back into view. If someone else saved first, their version loads and Undo brings yours back.

## How it works

| Thing | Behavior |
|---|---|
| Items | Time, duration, title, place, notes, roles. Sorted by time; times before 4 AM count as after midnight (so a 12:30 AM after-party sorts last). Gaps of 5 min or more show as "free". |
| Roles | Items with no roles are for everyone and show under every filter. Rename, recolor (click the swatch) or delete roles via **Edit roles**. |
| Now / Next | On the wedding day the panel shows what's on now (with time left) and what's next, past items fade, and the current item gets a progress bar. Before the day it shows a countdown. Refreshes every 30 s. |
| Print | Prints the timeline; if a role is selected, only that role's schedule, headed "Schedule for: …". |
| Start from a typical day | Asks when the day starts and ends (12:00–22:15 by default), then fills in a sample day with six roles fitted to that window. Getting ready, cocktail hour, dinner and dancing stretch or shrink; the ceremony, toasts, dances and other set pieces keep their length. A day shorter than about 6 hours shrinks everything evenly. Times round to 5 minutes. Undo removes it. |
| Same start time | Items that start at the same time share one row ("2 at once"), side by side on wide screens and stacked on phones. Now/Next lists all of them. |
| Keyboard | N adds an item, Esc closes dialogs. |
