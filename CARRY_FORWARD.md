# Ioniq 5 Colour Comparison — Carry Forward

## What this is

A static web app comparing the Hyundai Ioniq 5 2026 across three grades and three colours, with exterior angles, interior shots, and a spec summary page. Lives at `ioniq5.picturehooked.com`.

---

## Current state (as of June 2026)

### Working
- Three grades: Ultimate · N Line S · 5 N
- Three colours: Ecotronic Grey Pearl · Cyber Grey Metallic · Ult Red Metallic / Soultronic Orange Pearl (5 N)
- Compare toggle: **Colours** / **Trim / Model** — prominent pill-style segmented control
- **Colours mode** (internal: `compareMode === 'colours'`): pick a colour slate → all 3 models shown side by side in that colour. Panel labels = model names (Ultimate / N Line S / 5 N). Interior and Specs views included.
- **Trim / Model mode** (internal: `compareMode === 'models'`): pick a model slate → that model shown in all 3 colours side by side. Panel labels = colour names. Interior and Specs excluded (same image 3×).
- Specs page: final view in Colours mode only — side image dimmed behind per-model key spec list, gold bullets, 15% left offset
- Full circular navigation (both arrows cycle)
- Lightbox on tap with panel scroll and mobile portrait rotation
- Mobile fullscreen meta tags (active when added to home screen)
- Hyundai logo overlay on panels (hidden on specs view, hidden on mobile specs)
- 5N front images centre-cropped to match other models' framing (all 3 colours)
- Vercel Analytics enabled (script tag in `<head>`, toggled on in Vercel dashboard)

### Header layout (top to bottom)
1. Brand row: Hyundai Ioniq 5 · 2026 Current Model
2. Compare bar: COMPARE [Colours] [Trim / Model]
3. Selector area (dynamic, rebuilt by `renderSelector()`):
   - Colours mode: colour slates (Ecotronic / Cyber / Ult Red Metallic) — clicking sets `currentColour`
   - Trim/Model mode: model slates (Ultimate / N Line S / 5 N) — clicking sets `pickedModel` and reloads panels

### Important wiring note
Toggle buttons use `data-mode="models"` / `data-mode="colours"` attributes. Active class detection uses `t.dataset.mode === mode` — NOT `textContent` — so button labels can differ from internal mode strings. Do not remove the `data-mode` attributes.

### Spec items (as built)
**Ultimate**: 20" alloy wheels · Full leather seats · Digital Key / Head-up display · Full colour range, red standard · Zen Pack (opt): memory seats, vision roof · Tech Pack (opt): park assist, surround-view monitors

**N Line S**: 20" alloy wheels, upgraded design · Leather & alcantara panel seats · Zen & Tech Packs included as standard · Restricted colour range, no teal or blue

**5 N**: 21" alloy wheels, sporty design · Bucket alcantara seats · Vision roof optional · Limited colour range · Performance Blue available exclusively

### Image file structure
```
i5 Colours/
├── Ecotronic/       ← exterior jpegs, ecotronic colour
├── Cyber Grey/      ← exterior jpegs, cyber grey colour
├── red/             ← exterior jpegs, red colour
├── *.jpeg           ← interior shots (colour-agnostic): dash + seats per grade
├── hyundailogo.png  ← resized to 400px wide
├── index.html
├── DEPLOYMENT.md
└── CARRY_FORWARD.md
```

### Known filename quirks
- 5 N rear side images: `n back side [colour].jpeg` (not `rear side`)
- 5 N front images: zoom-cropped variants `n front [colour] zoom.jpeg` referenced in app. Originals retained.

---

## Pending / next round

- **Upgrade tier detail** — slide-up drawer per panel showing what each grade adds over the previous (delta view). Avoids a massive base list.
- **Additional colours** — add colour entry to `COLOURS` array + `renderSelector()` colour card + colour subfolder + update `images` map.
- Consider preloading next/prev images for smoother transitions at low connection speeds.

---

## Key decisions made

- Images served from Vercel CDN (same repo, no Supabase) — ~10MB total, well within limits
- Colour order fixed: Ecotronic → Cyber Grey → Red (matches slate order and lightbox panel order)
- Specs view: side image dimmed to 35%, gold bullet list starting 15% across, no performance figures
- Interior excluded from Trim/Model mode (same image 3× adds no value)
- Specs excluded from Trim/Model mode (only meaningful when comparing 3 models side by side)
- Mobile flex fix: `flex: 1 1 0` on `.model-panel` + JS reflow trigger (`display:none` / `offsetHeight` / `display:''`) in `setMode()` after `buildPanels()`
