# Ioniq 5 Colour Comparison — Carry Forward

## What this is

A static web app comparing the Hyundai Ioniq 5 2026 across three grades and three colours, with exterior angles, interior shots, and a spec summary page. Lives at `ioniq5.picturehooked.com` (subdomain pending DNS confirmation).

---

## Current state (as of June 2026)

### Working
- Three grades: Ultimate · N Line S · 5 N
- Three colours: Ecotronic Grey Pearl · Cyber Grey Metallic · Ultimate Red Metallic / Soultronic Orange Pearl (5 N)
- Seven views: Front · Front Side · Side · Rear Side · Rear · Dashboard · Seats
- Specs page: final view in models mode — side image dimmed behind per-model key spec list
- Compare toggle: Models mode (3 grades) / Colours mode (3 colours, 1 grade)
- Interior and Specs views excluded from Colours mode
- Full circular navigation (both arrows cycle)
- Lightbox on tap with model scroll and mobile portrait rotation
- Mobile fullscreen meta tags (active when added to home screen)
- Hyundai logo overlay on all panels
- 5N front images centre-cropped to match other models' framing (all 3 colours)

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
- 5 N rear side images are named `n back side [colour].jpeg` (not `rear side`)
- 5 N front images have zoom-cropped variants: `n front [colour] zoom.jpeg` — these are what the app references
- Original uncropped versions retained alongside zoomed copies

---

## Pending / next round

- Add more colours when available (add colour card in HTML + colour subfolder + update `images` map in `index.html`)
- "What's included" upgrade tier detail — suggested approach: delta view (what each grade adds over the previous), shown as a slide-up drawer per panel triggered by a small button. Avoids a massive base list.
- Consider preloading next/prev images for smoother transitions at low connection speeds
- Vercel URL to update in DEPLOYMENT.md once subdomain DNS confirms

---

## Key decisions made

- Images served from Vercel CDN (same repo, no Supabase) — images are ~10MB total, well within limits
- Colour order fixed: Ecotronic → Cyber Grey → Red (matches slate order and lightbox panel order)
- Specs view uses side image as background (dimmed to 35% overlay) with gold bullet list starting at 15% across
- No performance figures on spec page by design (key spec items only)
