# Phase 2: Theme Selection & Preview - Context

**Gathered:** 2026-03-19
**Status:** Ready for planning

<domain>
## Phase Boundary

Users can browse 8 preset visual themes, select one, and see a live carousel preview of their slides styled with that theme. Slide text cards (Phase 1 output) are replaced by the carousel preview. Export (PNG/ZIP download) is Phase 3. Custom theme editing is explicitly out of scope.

</domain>

<decisions>
## Implementation Decisions

### Theme Picker
- Horizontal scrollable swatch row displayed just above the carousel preview
- Each swatch shows a two-color strip: left half = background color, right half = accent color
- Selected state: gold border ring (#C8A96E) + bold theme name below the swatch
- Editorial theme is pre-selected automatically when a file loads (no explicit user action needed)
- All 8 themes shown in the row: Editorial, Nature/Botanical, Deep Space, Minimal, Sunset/Earth, Ocean, Botanical Dark, Tech/Code

### Preview Placement & Layout
- Carousel preview REPLACES the slide text cards entirely — preview is the primary view after upload
- Theme picker row sits immediately above the carousel preview
- Caption + hashtags sections remain visible below the carousel preview (needed for posting)
- Page flow after upload: filename pill + "Upload new" link → theme picker row → carousel preview → caption → hashtags

### Slide Render Fidelity
- Each slide rendered as an actual 1080×1080 div, CSS-scaled down to ~420px for display using `transform: scale(0.389)` and `transform-origin: top left`
- This ensures preview exactly matches export (PREV-03): same div, same fonts, same layout
- Slide content: topic/field badge at top, slide title (large, themed font), body text below
- Slide type variants:
  - Slide 1 (cover): large centered title, topic badge, field name — no body text column
  - Body slides (2 through N-1): title + body text layout
  - Last slide (CTA): bold call-to-action text ("Follow for daily science drops"), no body content column
- Theme variables applied per slide: background color, accent color, font families, accent element style

### Navigation UX
- Arrow buttons on left and right sides of the preview + dot indicators below showing position
- Left arrow disabled (grayed out) on slide 1; right arrow disabled on last slide — no wraparound
- Slide counter displayed (e.g., "2 / 6") above or below the preview
- Keyboard arrow keys (← →) navigate between slides
- Dots are clickable to jump directly to any slide

### Claude's Discretion
- Exact swatch size and spacing in the picker row
- Arrow button visual style (SVG icons, size, positioning relative to preview)
- Dot indicator size and active/inactive styling
- Transition animation between slides (crossfade, slide, or none)
- Exact font sizes within the 1080×1080 slide canvas
- Slide counter placement (above vs below preview)
- Overflow behavior of the swatch row on narrow viewports

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Theme definitions
- `.planning/PROJECT.md` — Full theme table with palette colors, vibe, and font pairings for all 8 themes (MANDATORY — this is the source of truth for theme specs)
- `.planning/REQUIREMENTS.md` — THEME-01, THEME-02, THEME-03, PREV-01, PREV-02, PREV-03 acceptance criteria

### Existing code
- `index.html` — Current app (Phase 1 complete): upload zone, parser, slide card renderer. Theme CSS variables and carousel preview will be added to this single file. Read current CSS structure before adding new styles.
- `solar_fuels_carousel_v2.html` — Reference implementation: 420×420 interactive carousel with dot navigation, prev/next arrows, warm cream theme, Lora serif titles, gold accent. Inspect for layout and animation patterns.

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- Upload zone + file handling: fully working in `index.html` — do not modify
- `parseMarkdown()` function: returns `{ title, date, field, slides[], caption, hashtags[] }` — use this data to populate the carousel
- `renderResults()`: currently renders slide text cards — this function will be replaced/extended to render the carousel preview instead
- CSS classes: `.results-header`, `.filename-pill`, `.upload-new-link`, `.caption-block`, `.hashtags-container`, `.hashtag-pill` — reuse these for the sections below the preview

### Established Patterns
- Single HTML file with all CSS in `<style>` block — no build step, no external JS files
- Vanilla JS only (ES5-compatible function syntax used throughout Phase 1)
- Safe DOM construction via `createElement/textContent` — do NOT use innerHTML (XSS prevention decision from Phase 1)
- Editorial theme colors already in CSS: background `#FAF8F4`, accent `#C8A96E`, body text `#4A4540`, heading `#1A1714`, muted `#9E8C6A`, border `#D0CCC4`
- Fonts loaded via Google Fonts CDN: Lora (serif) + Inter (sans) — additional theme fonts load dynamically or via CDN additions

### Integration Points
- `renderResults(parsed, filename)` is the entry point — after Phase 2 it should render the carousel UI instead of (or replacing) slide cards
- The `#results` div is the container for all post-upload content
- `uploadZone.style.display = 'none'` pattern already used to swap views — same pattern for carousel rendering

</code_context>

<specifics>
## Specific Ideas

- The `solar_fuels_carousel_v2.html` reference is the visual benchmark for the carousel preview — use it as the template for layout, dot navigation style, and overall feel
- The 1080×1080 div approach: a hidden `div.slide-canvas` at full resolution with `transform: scale(0.389)` displayed at 420px. The export phase (Phase 3) will use the same div at 1:1 scale via html2canvas.
- Theme switching should be instant (no loading state needed) — just update CSS custom properties or swap a data attribute on the canvas div

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 02-theme-selection-preview*
*Context gathered: 2026-03-19*
