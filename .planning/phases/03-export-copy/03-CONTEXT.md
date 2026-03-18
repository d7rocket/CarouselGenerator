# Phase 3: Export & Copy - Context

**Gathered:** 2026-03-19
**Status:** Ready for planning

<domain>
## Phase Boundary

Users can download their carousel as print-ready 1080×1080 PNGs (individual slide or full ZIP) and copy caption/hashtags to clipboard. The 1080×1080 `slide-canvas` divs from Phase 2 are reused directly — no new slide rendering. Caption and hashtags text blocks are already in the UI from Phase 2.

</domain>

<decisions>
## Implementation Decisions

### Export button placement
- Two export buttons sit below the carousel preview (above the caption block)
- **ZIP download is the primary CTA** — filled/solid button: "Download all slides as ZIP"
- **Single-slide download is secondary** — smaller/outlined button: "Download this slide only"
- "Download this slide" always exports whichever slide is currently visible in the carousel (current index)

### Copy button UX
- Clipboard icon button inlined at the top-right corner of each text block (caption block + hashtags block separately)
- Two independent copy buttons — one for caption, one for hashtags
- **Success feedback:** icon swaps to ✓ checkmark for ~2 seconds, then reverts to clipboard icon
- No toast, no layout shift, no labeled text — icon-only feedback

### Export feedback & states
- **During render (html2canvas processing):** clicked button disables and shows spinner + "Exporting..." text; other button remains normal
- **On success:** button returns to normal state; no toast or success banner — browser's native file download dialog is sufficient
- **On failure:** `alert()` with a short error message (e.g. "Export failed. Try reloading the page.")

### File naming
- Individual PNGs: `{topic-slug}-slide-01.png` where topic-slug is derived from the topic/field in the markdown header, lowercased and hyphenated (e.g. `solar-fuels-slide-01.png`)
- ZIP file: `{topic-slug}-carousel.zip` (e.g. `solar-fuels-carousel.zip`)
- Slides inside the ZIP use the same naming: `solar-fuels-slide-01.png`, `solar-fuels-slide-02.png`, etc.

### Claude's Discretion
- Exact spinner/loading animation style
- How topic slug is derived from the markdown title/field (remove special chars, replace spaces with hyphens, lowercase)
- Whether html2canvas or dom-to-image is used (html2canvas preferred per PROJECT.md)
- html2canvas render options (scale, useCORS, logging, etc.) for font fidelity
- Exact button visual styling (padding, border radius, icon SVGs) consistent with existing Phase 2 aesthetic

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Project constraints & requirements
- `.planning/PROJECT.md` — Tech stack (single HTML file, vanilla JS, CDN-only), export library decision (html2canvas or dom-to-image via CDN), JSZip via CDN for ZIP, and theme table
- `.planning/REQUIREMENTS.md` — EXPORT-01, EXPORT-02, EXPORT-03, EXPORT-04, COPY-01, COPY-02 acceptance criteria (exact requirements this phase must satisfy)

### Existing code
- `index.html` — Current app (Phases 1+2 complete): the `slide-canvas` divs at 1080×1080 with `transform: scale(0.389)`, `data-theme` attribute, carousel navigation, caption block, hashtags block. Read the full file before adding export code — all additions go into this single file.

No external specs — requirements fully captured in decisions above and REQUIREMENTS.md.

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.slide-canvas` divs: already rendered at 1080×1080 with correct theme styles — html2canvas should capture these directly (possibly from a hidden off-screen container or by temporarily removing the scale transform)
- `parseMarkdown()` return value includes `title` and `field` fields — use `field` (or `title` as fallback) to derive the topic slug for file naming
- Caption block (`.caption-block`) and hashtags container (`.hashtags-container`) — already in DOM with text content; copy button is added inline to these existing elements

### Established Patterns
- Single HTML file: all CSS in `<style>`, all JS in `<script>`, CDN libraries via `<script src>` tags in `<head>`
- Vanilla JS only, ES5-compatible function syntax
- Safe DOM construction via `createElement/textContent` — do NOT use innerHTML
- External libraries loaded via CDN `<script>` tags (Google Fonts, no module bundler)

### Integration Points
- `renderResults(parsed, filename)` is where the export UI should be injected — after the carousel navigation, before the caption block
- The `#results` div contains all post-upload content
- `currentSlideIndex` (or equivalent navigation state from Phase 2) must be readable to know which slide to export for single-slide download
- html2canvas / JSZip loaded via CDN `<script>` tags in `<head>` alongside existing Google Fonts links

</code_context>

<specifics>
## Specific Ideas

- The 1080×1080 canvas approach from Phase 2 context: "The export phase (Phase 3) will use the same div at 1:1 scale via html2canvas." — The plan is to temporarily remove the `transform: scale(0.389)` (or render from a hidden clone at 1:1) when capturing with html2canvas, not to re-render slides from scratch.
- File naming example from discussion: `solar-fuels-slide-01.png` inside `solar-fuels-carousel.zip`

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 03-export-copy*
*Context gathered: 2026-03-19*
