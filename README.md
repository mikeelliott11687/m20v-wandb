# Mooney M20 W&B & Performance (web)

Offline single-page calculator for Mooney **M20V Acclaim Ultra**, **M20TN**, **M20U Ovation Ultra**, **M20R Ovation**, **M20J 201** (2740 lb early / 2900 lb late), **M20K 231**, **M20K Encore**, and **M20M Bravo**.
Model toggle via `<select id="modelSelect">` — BEW / empty arm are never overwritten when switching.
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
4. Confirm the name (**Mooney W&B**) and tap **Add**.
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

## Models

| Model | Engine | Cruise | Front arms (in) | Notes |
|-------|--------|--------|-----------------|-------|
| **M20V** (default) | TSIO-550-G (5) turbo | FF tables ±0.5 gph/10°C | 37 / 40.25 / 43.5 | POH-003920 |
| **M20TN** | TSIO-550-G turbo | FF tables ±0.6 gph/10°C | 36 / 37 / 39 | Grass PENDING |
| **M20U** | IO-550-G (10) NA | MAP/% power ±0.4″/10°C | 36 / 37 / 43.5 | No grass charts |
| **M20R** | IO-550-G (6) NA | MAP/% power ±0.4″/10°C | 36 / 37 / 39 | No grass charts |

Extract packs: `/workspace/m20u/poh_extract/`, `/workspace/m20r/poh_extract/` (and prior m20v/m20tn packs).

## Features

### Weight & Balance (unchanged core)
- Inputs: BEW + empty arm; pilot/copilot + FWD/MID/AFT; rear L/R; fuel + density; **TKS fluid (CAV)** + density; baggage; hat rack; **Air conditioning** toggle (Sec VI AIR CONDITIONER MINUS WTS. → **66.1 lb @ 59.7 in**; all models incl. M20R)
- Arms: front 37 / 40.25 / 43.5; rear 73; fuel 49.23; TKS 70.7; baggage 101.5; hat rack 126
- TKS (optional, equip list 21J): 0–6.0 gal @ 9.2 lb/gal default → full 55.2 lb @ arm 70.7 in
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

## Optional equipment — Air conditioning
POH Sec VI **AIR CONDITIONER (MINUS WTS.)** (listed as 30.0 kg / 151.5 cm → **66.1 lb @ 59.7 in**): same figures in M20V / M20TN / M20U equipment lists.
Toggle **Air conditioning installed** (near the model selector) adds a W&B station when ON; unchecked excludes it (weight 0). Helper: *Add only if A/C weight is not already included in Basic Empty Weight.* Preference key `m20-wandb-ac` persists across model switches.
M20R POH Sec VI has no A/C line — toggle still offered with the same kit figures and a confirm-against-equipment-list note.
Each `MODEL_PROFILES` entry carries `ac: { available, weightLb: 66.1, armIn: 59.7, note }`.
**M20R:** no matching Sec VI equipment-list entry → toggle disabled.


## M20J notes

- Two GW toggles: **M20J_2740** and **M20J_2900**.
- **M20J_2740:** Manual **1229** ISSUED 9-6-83 (2740 lb). NORMAL landing **5-31** digitized (anchor 15°C/1500/2500/6HW → 860/1860). TO tabular from Manual 1221 family, validated vs 1229 nomogram 5-13. Cruise 5-20/5-21.
- **M20J_2900:** Manual 3203 Rev B — cruise/TO digitized; NORMAL paved landing **5-34** digitized (anchor 20°C/2000/2600/6HW → 975/2005); grass 5-35 anchors only.
- **Est. LOP HP = 13.9 × GPH** (training rule-of-thumb; not POH % power).
- BEW/empty arm never overwritten on model switch.


## M20K / M20M notes

- **M20K_231:** Manual **1228** ISSUED 9-4-81 / REV D 3-7-84 · S/N 25-0613–0780 · TSIO-360-GB · **2900 lb** TOW/LW · usable **75.6** / unusable 3 · CG 40.6@2360 / 43.5@2900 / 49.3@2900. Sec V cruise/TO/LDG **PENDING** digitization (scanned OCR).
- **M20K_Encore:** **POH-3303** OI 7-97 / Rev A 08-03-2010 · TSIO-360-SB · **3130 lb** · usable **75.6** / unusable 3 · CG includes **41.0@2430**. Sec V **PENDING** grids (figs 5-15 / 5-20 / 5-25).
- **M20M Bravo:** **POH/AFM 3501** OI 3-90 / Rev A 1-96 · TIO-540-AF1A/B · TOW **3368** / LW **3200** · usable **89** / unusable 6 · CG 41.0@2430 / 44@3300 / 46@3368 / 51@3368. Sec V **PENDING** grids (5-14 / 5-20 / 5-25).
- A/C and TKS unavailable by default for these three (not confirmed in extracts). LOP HP remains **13.9 × GPH**. BEW/empty arm never overwritten.

## Sec V status (2026-09-21)
| Model | Cruise | Paved TO | Paved LDG | Grass |
|-------|--------|----------|-----------|-------|
| M20K 231 | 5-19 done | 5-15 done | 5-48 done | 5-16/5-49 PENDING |
| M20K Encore | 5-20 done | 5-15 done | 5-25 done | % notes only PENDING |
| M20M Bravo | 5-20 done | 5-14 done | 5-25 done | 5-15/5-26 PENDING |
