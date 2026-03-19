# Roadmap: CarouselGenerator

## Overview

Three phases following the core value pipeline: parse markdown input, render it with selectable themes, then export as print-ready PNGs. Each phase delivers a complete, verifiable capability that builds on the previous one. The app is a single static HTML file with no build step.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [ ] **Phase 1: Markdown Input & Parsing** - User uploads a markdown file and the app extracts slides, caption, hashtags, and metadata
- [ ] **Phase 2: Theme Selection & Preview** - User picks from 8 visual themes and previews the carousel in-browser with real-time theme switching
- [ ] **Phase 3: Export & Copy** - User downloads slides as 1080x1080 PNGs (single or ZIP) and copies caption/hashtags

## Phase Details

### Phase 1: Markdown Input & Parsing
**Goal**: Users can load their markdown content and see it correctly parsed into individual slides with metadata
**Depends on**: Nothing (first phase)
**Requirements**: INPUT-01, INPUT-02, INPUT-03, INPUT-04, INPUT-05, INPUT-06
**Success Criteria** (what must be TRUE):
  1. User can drag a `.md` file onto the page and see it accepted
  2. User can click a file input to browse and select a `.md` file
  3. After upload, the app displays the correct number of parsed slides with their titles and body content
  4. Caption, hashtags, and field/topic metadata are extracted and accessible in the UI
**Plans**: 1 plan

Plans:
- [ ] 01-01-PLAN.md — Upload UI with drag-and-drop/file picker and markdown parser with inline validation tests

### Phase 2: Theme Selection & Preview
**Goal**: Users can browse visual themes, select one, and preview their carousel exactly as it will appear when exported
**Depends on**: Phase 1
**Requirements**: THEME-01, THEME-02, THEME-03, PREV-01, PREV-02, PREV-03
**Success Criteria** (what must be TRUE):
  1. User sees 8 distinct theme options and can select any one of them
  2. Selecting a theme immediately updates the slide preview without page reload
  3. User can navigate between slides using prev/next controls in the carousel preview
  4. The preview renders with the correct palette, typography, and accent style for each theme (matching the theme spec)
**Plans**: 2 plans

Plans:
- [ ] 02-01-PLAN.md — Theme CSS definitions (8 palettes), swatch picker, and carousel viewport with 1080x1080 slide rendering
- [ ] 02-02-PLAN.md — Carousel navigation controls (arrows, dots, keyboard, counter) and inline smoke tests

### Phase 3: Export & Copy
**Goal**: Users can download their carousel as print-ready PNGs and copy companion text for Instagram posting
**Depends on**: Phase 2
**Requirements**: EXPORT-01, EXPORT-02, EXPORT-03, EXPORT-04, COPY-01, COPY-02
**Success Criteria** (what must be TRUE):
  1. User can click to download any individual slide as a PNG that is exactly 1080x1080 pixels
  2. User can download all slides at once as a ZIP file containing correctly named PNGs
  3. Exported PNGs render text crisply with no visual artifacts or missing fonts
  4. User can copy caption text and hashtags to clipboard with a single click each
**Plans**: 2 plans

Plans:
- [ ] 03-01-PLAN.md — CDN setup (html2canvas + JSZip), export buttons with loading states, single-slide and ZIP download rendering
- [ ] 03-02-PLAN.md — Copy-to-clipboard buttons for caption and hashtags with icon feedback, plus end-to-end verification

## Progress

**Execution Order:**
Phases execute in numeric order: 1 -> 2 -> 3

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Markdown Input & Parsing | 0/1 | Planning complete | - |
| 2. Theme Selection & Preview | 0/2 | Planning complete | - |
| 3. Export & Copy | 0/2 | Planning complete | - |
