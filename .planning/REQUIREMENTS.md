# Requirements: CarouselGenerator

**Defined:** 2026-03-18
**Core Value:** Upload a markdown file → pick a theme → download print-ready 1080×1080 PNGs

## v1 Requirements

### Input

- [x] **INPUT-01**: User can drag and drop a `.md` file onto the app
- [x] **INPUT-02**: User can click to browse and upload a `.md` file
- [x] **INPUT-03**: App parses `## Slide N: Title` sections as individual slides (5–7 slides)
- [x] **INPUT-04**: App parses caption from `## Caption` section
- [x] **INPUT-05**: App parses hashtags from `## Hashtags` section
- [x] **INPUT-06**: App parses field/topic metadata from the header line

### Themes

- [ ] **THEME-01**: User can select from 8 preset visual themes before export
- [ ] **THEME-02**: Theme selection updates the preview in real-time
- [ ] **THEME-03**: Each theme has distinct palette, typography, and accent style:
  - Editorial (cream + gold, Lora+Inter) — default
  - Nature/Botanical (sage + leaf green)
  - Deep Space (near-black + electric blue)
  - Minimal (white + charcoal, hairline rules)
  - Sunset/Earth (sand + terracotta)
  - Ocean (slate + teal)
  - Botanical Dark (forest green + cream)
  - Tech/Code (dark + cyan, monospace)

### Preview

- [ ] **PREV-01**: App shows interactive carousel preview of all slides
- [ ] **PREV-02**: User can navigate between slides (prev/next)
- [ ] **PREV-03**: Preview accurately reflects the final exported design

### Export

- [ ] **EXPORT-01**: User can download a single slide as a 1080×1080 PNG
- [ ] **EXPORT-02**: User can download all slides as a ZIP of PNGs
- [ ] **EXPORT-03**: Exported PNGs are exactly 1080×1080 pixels
- [ ] **EXPORT-04**: Export quality is print-ready (fonts render crisp, no artifacts)

### Copy Utilities

- [ ] **COPY-01**: User can copy the caption text for the current slide with one click
- [ ] **COPY-02**: User can copy hashtags with one click

## v2 Requirements

### Customization

- **CUST-01**: User can override accent color within a theme
- **CUST-02**: User can toggle slide count (5 vs 6 vs 7)
- **CUST-03**: User can reorder slides before export

### Additional Formats

- **FMT-01**: Export as story format (1080×1920)
- **FMT-02**: Export as LinkedIn banner format

## Out of Scope

| Feature | Reason |
|---------|--------|
| Backend/server | Fully static, no data stored |
| AI content generation | User writes their own markdown |
| Custom theme editor | Preset themes sufficient for v1 |
| Mobile native app | Web-first |
| Real-time collaboration | Single-user tool |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| INPUT-01 | Phase 1: Markdown Input & Parsing | Complete |
| INPUT-02 | Phase 1: Markdown Input & Parsing | Complete |
| INPUT-03 | Phase 1: Markdown Input & Parsing | Complete |
| INPUT-04 | Phase 1: Markdown Input & Parsing | Complete |
| INPUT-05 | Phase 1: Markdown Input & Parsing | Complete |
| INPUT-06 | Phase 1: Markdown Input & Parsing | Complete |
| THEME-01 | Phase 2: Theme Selection & Preview | Pending |
| THEME-02 | Phase 2: Theme Selection & Preview | Pending |
| THEME-03 | Phase 2: Theme Selection & Preview | Pending |
| PREV-01 | Phase 2: Theme Selection & Preview | Pending |
| PREV-02 | Phase 2: Theme Selection & Preview | Pending |
| PREV-03 | Phase 2: Theme Selection & Preview | Pending |
| EXPORT-01 | Phase 3: Export & Copy | Pending |
| EXPORT-02 | Phase 3: Export & Copy | Pending |
| EXPORT-03 | Phase 3: Export & Copy | Pending |
| EXPORT-04 | Phase 3: Export & Copy | Pending |
| COPY-01 | Phase 3: Export & Copy | Pending |
| COPY-02 | Phase 3: Export & Copy | Pending |

**Coverage:**
- v1 requirements: 18 total
- Mapped to phases: 18
- Unmapped: 0

---
*Requirements defined: 2026-03-18*
*Last updated: 2026-03-18 after roadmap creation*
