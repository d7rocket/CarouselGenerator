# CarouselGenerator

## What This Is

A static web app (single HTML file) for generating Instagram carousel posts from a structured markdown file. Drop in a `.md` file, pick a visual theme, preview the slides, and download all 6 slides as 1080×1080 PNGs. Built for daily science/news content.

## Core Value

Upload a markdown file → pick a theme → download print-ready 1080×1080 PNGs. Everything else is secondary.

## Requirements

### Validated

- [x] User can drag/drop or upload a `.md` file matching the defined format — Validated in Phase 01: markdown-input-and-parsing
- [x] App parses slides from `## Slide N: Title` structure (5–7 slides) — Validated in Phase 01: markdown-input-and-parsing
- [x] Caption and hashtags parsed from markdown and copyable — Validated in Phase 01: markdown-input-and-parsing

### Validated

- [x] User can choose from 8 visual themes — Validated in Phase 02: theme-selection-preview
- [x] User can preview the carousel in-browser (1080×1080 scaled to 420px) — Validated in Phase 02: theme-selection-preview

### Active

- [ ] User can download each slide as a 1080×1080 PNG
- [ ] User can download all slides as a ZIP

### Out of Scope

- Backend/server — fully static, no uploads stored anywhere
- Mobile app — web only
- AI generation — user brings their own markdown
- Custom theme editor — themes are preset, not user-configurable in v1

## Context

- Existing reference: `solar_fuels_carousel_v2.html` — interactive carousel at 420×420 preview, warm cream (#FAF8F4) background, Lora serif titles, Inter body, gold accent (#C8A96E), corner SVG decorations, dot navigation
- Markdown format: `# Title`, metadata line, `## Slide N: Heading` sections with body text, `## Caption`, `## Hashtags`, `## Sources`, `## Images`
- Content scope: daily science facts/news — themes should suit scientific storytelling (nature, space, chemistry, biology, etc.)
- Export quality: 1080×1080 PNG using html2canvas or dom-to-image rendered from a hidden full-res slide div

## Constraints

- **Tech stack**: Vanilla HTML/CSS/JS, single file — matches existing HTML, no build step needed
- **Fonts**: Google Fonts CDN (Lora + Inter as baseline; themes may add other pairings)
- **Export**: Client-side only — html2canvas or dom-to-image via CDN
- **ZIP**: JSZip via CDN for bulk download

## Themes (planned)

8 preset themes, selectable before export:

| Theme | Palette | Vibe |
|-------|---------|------|
| Editorial (default) | Cream #FAF8F4, gold #C8A96E | Existing style — warm, journalistic |
| Nature / Botanical | Sage #F2F5EE, leaf green #6A8A5A | Biology, ecology, plants |
| Deep Space | Near-black #0E0F14, electric blue #4A9EFF | Astronomy, physics |
| Minimal | Pure white, charcoal #1A1A1A, hairline rules | Clean, general science |
| Sunset / Earth | Warm sand #FBF3E8, terracotta #C4602A | Geology, climate, energy |
| Ocean | Slate #F0F4F7, teal #2A8A8A | Marine biology, oceanography |
| Botanical Dark | Forest green #0D1F0F, cream text | Striking dark variant |
| Tech / Code | Dark #111418, cyan #00D4AA, monospace | Computer science, AI |

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Single HTML file | Matches existing workflow, no build tooling needed | Confirmed — all code in index.html |
| html2canvas for export | Browser-native, no server required | — Pending (Phase 3) |
| 8 preset themes | User wants variety without a custom editor | Shipped — all 8 themes live with CSS custom properties |

---
*Last updated: 2026-03-19 — Phase 02 complete: 8 themes, swatch picker, carousel preview, navigation controls shipped*
