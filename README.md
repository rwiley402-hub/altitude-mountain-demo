# Altitude — Mountain Platform demo

An interactive pitch demo for the Mountain Tourism Complex Platform MVP
(`Mountain_MVP_Scope_v1.pdf`). One page, no build step, no backend.

It exists to answer the three questions the client asked in the bid thread, on screen
rather than in prose:

1. **One folio, correct per-outlet revenue.** A three-night half-board stay with charges
   from six outlets. Void the ski lesson for wind and the ski school drops to €0.00 while
   every other outlet is untouched; settle half card and half cash and no outlet total moves.
2. **Weather per altitude.** Six weather points from 1,050 m to 2,600 m. Drag the gust limit
   below the forecast and warnings cascade onto the matching lifts, shuttle trips and outdoor
   slots — the `done when` check from scope section H, running live.
3. **The shape of the build.** RLS, append-only money, VAT snapshots per line, CI/CD, hosting.

## Run it

```
# any static server; the page is a single file plus assets
cd mountain-demo
python -m http.server 8080
# then open http://localhost:8080
```

Opening `index.html` directly from the filesystem also works in Chrome and Firefox.

## Files

```
index.html          the whole demo — markup, CSS, JS
assets/img/*.jpg    26 photographs + 5 video poster frames
assets/video/*.mp4  5 clips
```

`index.html` is written to publish as a claude.ai Artifact unchanged, so it carries no
`<!doctype>`/`<html>`/`<head>` wrapper. Browsers supply those automatically.

## What drives what

| Thing | Where |
|---|---|
| 3D ridge | `three.js` 0.160.0 from cdnjs, procedural value-noise terrain, flat-shaded |
| Station markers | placed by scanning the terrain for the x where height matches each real altitude |
| Folio engine | `FOLIO` IIFE — `charges`, `voids`, `payments` as three append-only arrays, all in cents |
| Weather wall | `STATIONS` seed data; gust dials drawn on canvas |
| Film grain, snowfall | canvas, ~12 fps noise and a drifting particle field |
| Everything else | hand-written CSS and vanilla JS, no framework |

Money is integer cents throughout. VAT uses Austrian rates (accommodation and food 10 %,
lift passes 13 %, drinks and services 20 %) and each line stores its own net/tax/gross
snapshot, as the scope requires.

## Asset sources

All assets are free for commercial use and were downloaded, not hotlinked.

- Photographs — [Unsplash](https://unsplash.com), Unsplash License
- Footage — [Pexels](https://www.pexels.com), Pexels License
- Typefaces — Bodoni Moda, Archivo, IBM Plex Mono (Google Fonts, SIL OFL)
- `three.js` — MIT

The page credits MET Norway and OpenStreetMap in the footer because the real product
calls those APIs. **The demo itself makes no network calls to them** — every forecast on
screen is seeded sample data shaped like a `Locationforecast 2.0` response. Nothing here
needs an API key.

## Known limits

- Single dark theme on purpose; it is a night-mountain piece.
- The 3D view pauses when scrolled out of sight and respects `prefers-reduced-motion`.
- Video autoplay is muted, as every browser requires.
