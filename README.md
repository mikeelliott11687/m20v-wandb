# M20V Acclaim Ultra — W&B & Performance (web)

Offline single-page calculator for the Mooney M20V Acclaim Ultra.
Companion / fallback to the Open as App workbook.

**Core:** `index.html` (self-contained HTML + CSS + JS — no build step), plus PWA assets (`manifest.webmanifest`, `sw.js`, `icons/`).

## Open locally

1. Open `index.html` in any modern browser (Chrome, Safari, Firefox, Edge).
2. Or from this folder:

```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Or serve (optional — not required; file:// works)
python3 -m http.server 8080
# then visit http://localhost:8080/
```

Works fully offline once loaded (service worker caches the page, manifest, and icons). Prefer serving over **https** (or localhost) so the service worker and home-screen install work — `file://` may open the calculator but will not register the SW.

## Install on iPhone / iPad (Safari) — primary

iOS uses “Add to Home Screen” (not Chrome’s install prompt):

1. Open the app URL in **Safari** (not Chrome/Firefox on iOS).
2. Tap the **Share** button (square with ↑).
3. Scroll and tap **Add to Home Screen**.
4. Confirm the name (**M20V W&B**) and tap **Add**.
5. Launch from the home-screen icon — it opens full-screen (standalone) thanks to `apple-mobile-web-app-capable`.

Notes: Use a real https host (e.g. GitHub Pages). After updates, reopen once online so the service worker can refresh the cache. `viewport-fit=cover` supports notched devices.

## Install on Android (secondary)

1. Open the URL in **Chrome**.
2. Use the menu → **Install app** / **Add to Home screen**, or the install banner if shown.
3. Or: menu → **Add to Home screen**.

The web app manifest (`manifest.webmanifest`) and service worker support offline use after the first visit.

## PWA files

| File | Role |
|------|------|
| `manifest.webmanifest` | Name, theme, icons, `display: standalone` |
| `sw.js` | Cache-first offline for `index.html`, manifest, icons |
| `icons/icon-192.png`, `icons/icon-512.png` | Android / manifest icons |
| `icons/apple-touch-icon.png` | 180×180 iOS home-screen icon |

Paths are relative (`./`) so GitHub Pages works at repo root or a subpath like `/m20v-wandb/`.

## Features

### Weight & Balance (unchanged core)
- Inputs: BEW + empty arm; pilot/copilot + FWD/MID/AFT; rear L/R; fuel + density; baggage; hat rack
- Arms: front 37 / 40.25 / 43.5; rear 73; fuel 49.23; baggage 101.5; hat rack 126
- Outputs: station table, total weight, CG, useful load, CG envelope plot
- Limits: max TOW **3368 lb**, MLW **3200 lb**, baggage 120, hat rack 10
- Dual units: lb/kg, in/mm, gal/litres

### Performance (new)
| Section | Live | Pending |
|---------|------|---------|
| **Takeoff** | Recommended TO / 50′ speeds; **ground roll & over-50′** from digitized Figs 5-17 / 5-18 (weight×PA×OAT + wind); surface factor optional non-POH | Runway gradient (charts support it; app assumes 0%) |
| **Landing** | Sec IV speeds; chart approach/brake; **ground roll & over-50′** from digitized Figs 5-71 / 5-72 + wind | Gradient (assumed 0%); +100 ft/kt over approach speed (advisory only) |
| **Cruise fuel flow** | Best Power & Best Economy tables (POH Sec V 5-23/5-24) with altitude interpolation and ±0.5 gph / 10°C ISA correction; RPM/MP selectors; TIT / Note 1–2 guards | TAS / range / endurance charts |

- Link: “Use TOW in Performance” / “Fill weight from W&B”
- Prominent AFM/POH disclaimer on every view

## Digitized distances
- Source tables: `../poh_extract/digitized_distances.json` (also embedded as `DIST` in `index.html`)
- Method / caveats: `../poh_extract/digitize_notes.md`
- Typical uncertainty ±50–100 ft; **AFM/POH nomogram governs**

## Still pending
- Runway gradient input (charts have fans; app currently assumes 0%)
- Climb, TAS, range, endurance performance

## Disclaimer

Training / planning aid only. The AFM/POH governs. Takeoff/landing distances are **digitized approximations** of POH nomograms (±50–100 ft typical); verify critical field performance on the paper charts. Use your aircraft-specific empty weight and arm.

## Specs source

`m20v_build_notes.md`, `m20v_values_to_update.md`, `poh_extract/cruise_power_tables.txt`, Excel `Cruise FF Tables` sheet, POH Sec II / IV / V / VI.

## Google Sites embed (mooneypros.com)

The calculator is embedded as a “whole page” URL embed. On iPhone, a short Sites
aspect-ratio box left a large blank region under a ~200px-tall iframe.

This app posts Google Sites’ `SHIC` resize messages so the embed can grow to the
content height. If a blank band remains after publish/cache refresh, open the
Sites editor → select the embed → set the aspect ratio to a tall/portrait size
(or the tallest custom ratio), then republish.
