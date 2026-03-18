# Phase 2: Theme Selection & Preview - Research

**Researched:** 2026-03-19
**Domain:** CSS theming, carousel UI, vanilla JS DOM rendering
**Confidence:** HIGH

## Summary

Phase 2 transforms the Phase 1 slide-card output into a full interactive carousel preview with 8 switchable visual themes. The architecture is straightforward: CSS custom properties on a `[data-theme]` attribute selector provide instant theme switching, a 1080x1080 div scaled via `transform: scale(0.389)` gives pixel-perfect preview fidelity (matching Phase 3 export), and vanilla JS handles carousel navigation with prev/next arrows, dot indicators, and keyboard support.

The reference implementation (`solar_fuels_carousel_v2.html`) already demonstrates the exact carousel pattern needed: absolute-positioned slides with opacity/translateX transitions, dot navigation, and prev/next controls. This code can be adapted directly. The main new work is: (1) defining 8 theme palettes as CSS custom property sets, (2) building a swatch picker row, (3) generating slide DOM dynamically from `parseMarkdown()` output, and (4) handling three slide type variants (cover, body, CTA).

**Primary recommendation:** Use `data-theme` attribute on the slide canvas container with CSS custom properties for all theme-variable colors/fonts. Build the carousel as a direct adaptation of the reference implementation's navigation pattern. Load all theme fonts upfront via a single Google Fonts `@import`.

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
- Horizontal scrollable swatch row displayed just above the carousel preview
- Each swatch shows a two-color strip: left half = background color, right half = accent color
- Selected state: gold border ring (#C8A96E) + bold theme name below the swatch
- Editorial theme is pre-selected automatically when a file loads
- All 8 themes shown in the row: Editorial, Nature/Botanical, Deep Space, Minimal, Sunset/Earth, Ocean, Botanical Dark, Tech/Code
- Carousel preview REPLACES the slide text cards entirely -- preview is the primary view after upload
- Theme picker row sits immediately above the carousel preview
- Caption + hashtags sections remain visible below the carousel preview
- Page flow after upload: filename pill + "Upload new" link -> theme picker row -> carousel preview -> caption -> hashtags
- Each slide rendered as an actual 1080x1080 div, CSS-scaled down to ~420px for display using `transform: scale(0.389)` and `transform-origin: top left`
- Slide content: topic/field badge at top, slide title (large, themed font), body text below
- Slide type variants: cover (slide 1), body slides (2 through N-1), CTA (last slide)
- Theme variables applied per slide: background color, accent color, font families, accent element style
- Arrow buttons on left and right sides of the preview + dot indicators below showing position
- Left arrow disabled (grayed out) on slide 1; right arrow disabled on last slide -- no wraparound
- Slide counter displayed (e.g., "2 / 6") above or below the preview
- Keyboard arrow keys navigate between slides
- Dots are clickable to jump directly to any slide

### Claude's Discretion
- Exact swatch size and spacing in the picker row
- Arrow button visual style (SVG icons, size, positioning relative to preview)
- Dot indicator size and active/inactive styling
- Transition animation between slides (crossfade, slide, or none)
- Exact font sizes within the 1080x1080 slide canvas
- Slide counter placement (above vs below preview)
- Overflow behavior of the swatch row on narrow viewports

### Deferred Ideas (OUT OF SCOPE)
None -- discussion stayed within phase scope.
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| THEME-01 | User can select from 8 preset visual themes before export | Swatch picker row with `data-theme` attribute switching; all 8 theme palettes defined as CSS custom property sets |
| THEME-02 | Theme selection updates the preview in real-time | `data-theme` attribute swap on canvas container instantly applies new CSS custom properties -- no reload needed |
| THEME-03 | Each theme has distinct palette, typography, and accent style | Full palette table below with hex values, font pairings, and accent element descriptions per theme |
| PREV-01 | App shows interactive carousel preview of all slides | 1080x1080 slide divs scaled to ~420px, dynamically generated from parseMarkdown() output, with navigation controls |
| PREV-02 | User can navigate between slides (prev/next) | Prev/next arrow buttons, clickable dot indicators, keyboard arrow keys, slide counter |
| PREV-03 | Preview accurately reflects the final exported design | Same 1080x1080 div used for both preview (scaled) and export (1:1) -- pixel-perfect fidelity by design |
</phase_requirements>

## Standard Stack

### Core
| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Google Fonts CDN | N/A | Font loading for all 8 theme pairings | Already used in Phase 1; standard approach for static sites |
| CSS Custom Properties | Native | Theme variable system (`--bg`, `--accent`, `--font-heading`, etc.) | Zero-dependency, instant swap, works in all modern browsers |
| Vanilla JS (ES5) | Native | DOM generation, carousel navigation, theme switching | Project constraint -- single HTML file, no build step |

### Supporting
| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| None | - | - | Everything is native CSS/JS for this phase |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| CSS custom properties | CSS class swapping per element | Custom properties are cleaner -- one attribute change cascades everywhere |
| `transform: scale()` for preview | Duplicate CSS at 420px sizes | Scale approach guarantees export fidelity (PREV-03); duplicate CSS would drift |

**Installation:**
No installation needed. All fonts loaded via Google Fonts CDN `@import` in the existing `<style>` block.

## Architecture Patterns

### Recommended Structure (within single HTML file)

```
<style>
  /* Existing Phase 1 styles... */

  /* Theme definitions: [data-theme="editorial"], [data-theme="nature"], etc. */
  /* Swatch picker styles */
  /* Slide canvas styles (1080x1080) */
  /* Carousel navigation styles */
  /* Slide type variants (cover, body, CTA) */
</style>

<div id="results">
  <!-- Dynamically built by renderResults(): -->
  <!-- 1. Header row (filename pill + upload new) -->
  <!-- 2. Theme picker swatch row -->
  <!-- 3. Carousel viewport (420px visible area) -->
  <!--    └── Slide canvas container [data-theme="editorial"] -->
  <!--        └── Individual 1080x1080 slide divs -->
  <!-- 4. Navigation controls (arrows + counter + dots) -->
  <!-- 5. Caption block -->
  <!-- 6. Hashtags container -->
</div>
```

### Pattern 1: CSS Custom Properties for Theme Switching

**What:** Define all theme-variable colors and fonts as CSS custom properties scoped to `[data-theme="X"]` selectors on the canvas container.

**When to use:** Always -- this is the core theming mechanism.

**Example:**
```css
/* Default / Editorial theme */
.slide-canvas {
  --slide-bg: #FAF8F4;
  --slide-accent: #C8A96E;
  --slide-heading-color: #1A1714;
  --slide-body-color: #4A4540;
  --slide-muted: #9E8C6A;
  --slide-border: #D0CCC4;
  --slide-font-heading: 'Lora', serif;
  --slide-font-body: 'Inter', sans-serif;
}

/* Deep Space theme */
.slide-canvas[data-theme="deep-space"] {
  --slide-bg: #0E0F14;
  --slide-accent: #4A9EFF;
  --slide-heading-color: #E8ECF2;
  --slide-body-color: #A0A8B8;
  --slide-muted: #5A6478;
  --slide-border: #1E2230;
  --slide-font-heading: 'Space Grotesk', sans-serif;
  --slide-font-body: 'Inter', sans-serif;
}
```

**JS theme switch (one line):**
```javascript
document.querySelector('.slide-canvas').setAttribute('data-theme', themeName);
```

### Pattern 2: 1080x1080 Scaled Preview

**What:** Render each slide at full 1080x1080 resolution inside a container that uses `transform: scale(0.389)` to display at ~420px. The container has `overflow: hidden` and explicit width/height of 420px.

**When to use:** For the carousel viewport.

**Example:**
```css
.carousel-viewport {
  width: 420px;
  height: 420px;
  position: relative;
  overflow: hidden;
  border-radius: 4px;
  border: 0.5px solid #D0CCC4;
}

.slide-frame {
  width: 1080px;
  height: 1080px;
  transform: scale(0.389);
  transform-origin: top left;
  position: absolute;
  top: 0;
  left: 0;
}
```

### Pattern 3: Carousel Navigation (adapted from reference)

**What:** Absolute-positioned slide divs with opacity+translateX transitions. Only the active slide is visible.

**Example:**
```css
.slide-frame {
  opacity: 0;
  transform: scale(0.389) translateX(60px);
  transition: opacity 0.35s ease, transform 0.35s ease;
  pointer-events: none;
}

.slide-frame.active {
  opacity: 1;
  transform: scale(0.389) translateX(0);
  pointer-events: auto;
}
```

```javascript
function goToSlide(idx) {
  var slides = document.querySelectorAll('.slide-frame');
  slides[currentSlide].classList.remove('active');
  currentSlide = idx;
  slides[currentSlide].classList.add('active');
  updateCounter();
  updateDots();
  updateArrowStates();
}
```

### Pattern 4: Slide Type Variants

**What:** Three distinct slide layouts generated based on position in the slides array.

**Cover slide (slide 1):** Large centered title, topic badge, field name. No body text column.
**Body slides (2 through N-1):** Title + body text layout with accent rule.
**CTA slide (last):** Bold call-to-action text, no body content column.

**Detection logic:**
```javascript
function getSlideType(index, totalSlides) {
  if (index === 0) return 'cover';
  if (index === totalSlides - 1) return 'cta';
  return 'body';
}
```

### Anti-Patterns to Avoid
- **innerHTML for slide content:** The project uses safe DOM construction (createElement/textContent). Do NOT use innerHTML even though the reference file does -- this was a Phase 1 security decision.
- **Separate CSS files or JS modules:** Everything stays in the single HTML file. No external stylesheets.
- **Hardcoded slide count:** The carousel must work with 5-7 slides dynamically based on parsed markdown.
- **Wrapping carousel navigation:** User decided left arrow disabled on slide 1, right arrow disabled on last slide -- no wraparound.

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Font loading | Custom font-face declarations | Google Fonts CDN `@import` | Handles subsetting, caching, CORS, format negotiation |
| Theme variable cascading | Per-element style overrides | CSS custom properties with `[data-theme]` | One attribute change cascades to all children |
| Slide transition animation | Custom requestAnimationFrame loops | CSS transitions on opacity/transform | Hardware-accelerated, zero JS overhead |

## Common Pitfalls

### Pitfall 1: Transform Scale Breaks Absolute Positioning
**What goes wrong:** Child elements positioned with `position: absolute` inside a scaled parent can have unexpected coordinates because transforms create a new stacking context.
**Why it happens:** `transform: scale()` affects the coordinate space of children.
**How to avoid:** Keep the scale on the outermost slide frame div. Internal slide elements should use normal flow or absolute positioning relative to the unscaled 1080x1080 space. The viewport container (420x420) clips the output.
**Warning signs:** Elements appearing outside the visible viewport area, click targets misaligned.

### Pitfall 2: Font Loading Flash (FOUT)
**What goes wrong:** Slides render with fallback fonts briefly before Google Fonts load, causing layout shifts.
**Why it happens:** Google Fonts are loaded asynchronously.
**How to avoid:** Use `@import` in the `<style>` block (already done in Phase 1 for Lora+Inter). Add all additional theme fonts to the same `@import` URL. The `display=swap` parameter is already in use. Since the carousel only renders after user uploads a file, fonts will almost certainly be loaded by then.
**Warning signs:** Text reflowing after initial render.

### Pitfall 3: Transition Conflicts with Transform Scale
**What goes wrong:** Animating both `opacity` and `transform` when the base transform already includes `scale(0.389)` can cause the scale to reset during animation.
**Why it happens:** CSS transitions interpolate from the computed value. If the active state only specifies `translateX(0)`, the scale portion may be lost.
**How to avoid:** Always include the scale in every transform state: `transform: scale(0.389) translateX(0)` for active, `transform: scale(0.389) translateX(60px)` for inactive.
**Warning signs:** Slides briefly appearing at full 1080px size during transitions.

### Pitfall 4: Swatch Picker Horizontal Overflow
**What goes wrong:** On narrow viewports, 8 swatches may overflow the container.
**Why it happens:** Fixed-width swatches exceed the viewport width.
**How to avoid:** Use `overflow-x: auto` with `-webkit-overflow-scrolling: touch` on the swatch container. Consider `flex-shrink: 0` on individual swatches.
**Warning signs:** Swatches being cut off or wrapping to a second row.

### Pitfall 5: Keyboard Event Conflicts
**What goes wrong:** Arrow key listeners fire when user is typing in another part of the page (if any input exists).
**Why it happens:** Global `keydown` listeners catch all arrow key events.
**How to avoid:** Only listen for arrow keys when the carousel is visible (results view is active). No text inputs exist in the results view, so this is low risk but worth checking.
**Warning signs:** Unexpected slide changes when interacting with other UI elements.

## Code Examples

### Full Google Fonts Import (all 8 themes)
```css
@import url('https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;1,400&family=Inter:wght@300;400;500&family=Playfair+Display:wght@400;600&family=Source+Sans+3:wght@300;400;500&family=Space+Grotesk:wght@400;500;600&family=Space+Mono:wght@400;700&family=DM+Sans:wght@300;400;500&family=Bitter:wght@400;600&family=JetBrains+Mono:wght@400;500&display=swap');
```

### Theme Palette Definitions (all 8 themes)

Source: `.planning/PROJECT.md` theme table + CONTEXT.md decisions.

| Theme | data-theme | Background | Accent | Heading Color | Body Color | Muted | Border | Heading Font | Body Font | Accent Style |
|-------|-----------|------------|--------|---------------|------------|-------|--------|-------------|-----------|--------------|
| Editorial | `editorial` | #FAF8F4 | #C8A96E | #1A1714 | #4A4540 | #9E8C6A | #D0CCC4 | Lora, serif | Inter, sans-serif | Gold rule + corner SVG |
| Nature/Botanical | `nature` | #F2F5EE | #6A8A5A | #1A2E14 | #3A4A34 | #7A9A6A | #C8D4C0 | Bitter, serif | Source Sans 3, sans-serif | Leaf green rule |
| Deep Space | `deep-space` | #0E0F14 | #4A9EFF | #E8ECF2 | #A0A8B8 | #5A6478 | #1E2230 | Space Grotesk, sans-serif | Inter, sans-serif | Electric blue glow rule |
| Minimal | `minimal` | #FFFFFF | #1A1A1A | #1A1A1A | #4A4A4A | #8A8A8A | #E0E0E0 | DM Sans, sans-serif | DM Sans, sans-serif | Hairline 1px charcoal rule |
| Sunset/Earth | `sunset` | #FBF3E8 | #C4602A | #2A1A0A | #5A4030 | #A07850 | #D8C8B0 | Playfair Display, serif | Source Sans 3, sans-serif | Terracotta rule |
| Ocean | `ocean` | #F0F4F7 | #2A8A8A | #0A2A2A | #2A4A4A | #5A8A8A | #C0D4D4 | DM Sans, sans-serif | Inter, sans-serif | Teal rule |
| Botanical Dark | `botanical-dark` | #0D1F0F | #D4C9A8 | #F2EFE4 | #C8C0A8 | #8A8A6A | #1A3A1A | Playfair Display, serif | Source Sans 3, sans-serif | Cream/gold rule on dark |
| Tech/Code | `tech` | #111418 | #00D4AA | #E0E8F0 | #A0B0C0 | #5A6A7A | #1A2028 | JetBrains Mono, monospace | Space Grotesk, sans-serif | Cyan glow rule |

### Swatch Picker Construction
```javascript
function buildThemePicker(container, onSelect) {
  var themes = [
    { id: 'editorial', name: 'Editorial', bg: '#FAF8F4', accent: '#C8A96E' },
    { id: 'nature', name: 'Nature', bg: '#F2F5EE', accent: '#6A8A5A' },
    { id: 'deep-space', name: 'Deep Space', bg: '#0E0F14', accent: '#4A9EFF' },
    { id: 'minimal', name: 'Minimal', bg: '#FFFFFF', accent: '#1A1A1A' },
    { id: 'sunset', name: 'Sunset', bg: '#FBF3E8', accent: '#C4602A' },
    { id: 'ocean', name: 'Ocean', bg: '#F0F4F7', accent: '#2A8A8A' },
    { id: 'botanical-dark', name: 'Botanical Dark', bg: '#0D1F0F', accent: '#D4C9A8' },
    { id: 'tech', name: 'Tech', bg: '#111418', accent: '#00D4AA' }
  ];

  var row = document.createElement('div');
  row.className = 'theme-picker-row';

  themes.forEach(function(theme) {
    var swatch = document.createElement('button');
    swatch.className = 'theme-swatch';
    swatch.setAttribute('data-theme-id', theme.id);

    // Two-color strip
    var leftHalf = document.createElement('div');
    leftHalf.className = 'swatch-half swatch-bg';
    leftHalf.style.backgroundColor = theme.bg;
    var rightHalf = document.createElement('div');
    rightHalf.className = 'swatch-half swatch-accent';
    rightHalf.style.backgroundColor = theme.accent;
    swatch.appendChild(leftHalf);
    swatch.appendChild(rightHalf);

    // Theme name label
    var label = document.createElement('span');
    label.className = 'swatch-label';
    label.textContent = theme.name;
    swatch.appendChild(label);

    swatch.addEventListener('click', function() {
      onSelect(theme.id);
    });

    row.appendChild(swatch);
  });

  container.appendChild(row);
}
```

### Slide DOM Generation (safe construction)
```javascript
function buildSlideDOM(slide, index, totalSlides, field) {
  var type = getSlideType(index, totalSlides);
  var frame = document.createElement('div');
  frame.className = 'slide-frame' + (index === 0 ? ' active' : '');
  frame.setAttribute('data-slide-index', index);

  // Top row: field badge + slide counter
  var topRow = document.createElement('div');
  topRow.className = 'slide-top-row';
  var badge = document.createElement('span');
  badge.className = 'slide-badge';
  badge.textContent = field || 'Science Drop';
  topRow.appendChild(badge);
  var num = document.createElement('span');
  num.className = 'slide-num';
  num.textContent = String(index + 1).padStart(2, '0') + ' \u2014 ' + String(totalSlides).padStart(2, '0');
  topRow.appendChild(num);
  frame.appendChild(topRow);

  // Body area
  var body = document.createElement('div');
  body.className = 'slide-body slide-type-' + type;
  var title = document.createElement('div');
  title.className = 'slide-title';
  title.textContent = slide.title;
  body.appendChild(title);

  // Accent rule
  var rule = document.createElement('div');
  rule.className = 'slide-accent-rule';
  body.appendChild(rule);

  // Body text (not on cover or CTA)
  if (type === 'body') {
    var text = document.createElement('div');
    text.className = 'slide-text';
    text.textContent = slide.body;
    body.appendChild(text);
  } else if (type === 'cta') {
    var cta = document.createElement('div');
    cta.className = 'slide-cta-text';
    cta.textContent = slide.body || 'Follow for daily science drops.';
    body.appendChild(cta);
  }

  frame.appendChild(body);
  return frame;
}
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Class-based theme toggling | CSS custom properties + data attributes | ~2020+ (widespread) | Single attribute change cascades all theme vars |
| Duplicate CSS at display size | `transform: scale()` from full-res | Standard practice | Guarantees export fidelity, simplifies maintenance |
| jQuery carousel plugins | Vanilla JS with CSS transitions | ~2018+ | No dependencies, better performance |

**Deprecated/outdated:**
- jQuery-based carousels (Slick, Owl): unnecessary for this simple use case and would violate the no-dependencies constraint

## Open Questions

1. **Exact font pairings per theme**
   - What we know: Editorial uses Lora+Inter (confirmed in Phase 1). PROJECT.md specifies vibe per theme but not exact font names for all 8.
   - What's unclear: The specific serif/sans/mono assignments for Nature, Ocean, Sunset, Botanical Dark themes are my best-fit recommendations based on vibe.
   - Recommendation: The pairings in the theme table above are sensible defaults. Bitter+Source Sans 3 for Nature/earthy themes, Playfair Display for elegant themes, Space Grotesk/JetBrains Mono for tech. These can be adjusted during implementation if they don't feel right visually.

2. **Accent element style per theme**
   - What we know: Editorial has gold corner SVGs and accent rule (from reference). CONTEXT.md says "accent element style" varies per theme.
   - What's unclear: Whether each theme gets unique decorative elements (SVG corners, borders, etc.) or just color-swapped versions of the same element.
   - Recommendation: Start with the same accent rule element (horizontal bar under title) color-swapped per theme. Corner SVGs are Editorial-specific. Other themes can have simpler or no corner decorations. This keeps scope manageable.

## Validation Architecture

### Test Framework
| Property | Value |
|----------|-------|
| Framework | Browser-based inline tests (same pattern as Phase 1 parser tests) |
| Config file | None -- tests are inline in `<script>` block |
| Quick run command | Open `index.html` in browser, check console for test output |
| Full suite command | Open `index.html` in browser, check console for all PASSED messages |

### Phase Requirements -> Test Map
| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| THEME-01 | 8 theme swatches rendered, click selects theme | manual | Visual check: 8 swatches visible, click changes data-theme attribute | No -- Wave 0 |
| THEME-02 | Theme switch updates preview without reload | manual | Click each swatch, verify no page reload, colors change instantly | No -- Wave 0 |
| THEME-03 | Each theme has distinct palette/typography/accent | manual + smoke | Console test: verify all 8 data-theme values produce different computed CSS variable values | No -- Wave 0 |
| PREV-01 | Interactive carousel preview shows all slides | manual + smoke | Console test: verify N slide-frame elements exist matching parsed slides count | No -- Wave 0 |
| PREV-02 | Prev/next navigation works | manual + smoke | Console test: goToSlide(1) changes active class; arrow buttons exist | No -- Wave 0 |
| PREV-03 | Preview matches export design | manual | Visual check: 1080x1080 div with transform scale, same div used for both | No -- Wave 0 |

### Sampling Rate
- **Per task commit:** Open index.html, upload test markdown, visually verify + check console
- **Per wave merge:** Full manual walkthrough of all 8 themes + navigation
- **Phase gate:** All 8 themes render correctly, all navigation works, console shows no errors

### Wave 0 Gaps
- [ ] Inline smoke test function `runCarouselTests()` -- verifies DOM structure: 8 swatches, correct slide count, data-theme attribute present, navigation functions exist
- [ ] Test markdown file for consistent manual testing (can reuse the Phase 1 inline test markdown)

## Sources

### Primary (HIGH confidence)
- `index.html` -- Current Phase 1 implementation, directly inspected for CSS structure, DOM patterns, and integration points
- `solar_fuels_carousel_v2.html` -- Reference carousel implementation, directly inspected for navigation pattern, slide layout, transition CSS, and dot indicator approach
- `.planning/PROJECT.md` -- Theme palette table (background colors, accent colors, vibes)
- `.planning/REQUIREMENTS.md` -- THEME-01/02/03, PREV-01/02/03 acceptance criteria
- `.planning/phases/02-theme-selection-preview/02-CONTEXT.md` -- All locked decisions and discretion areas

### Secondary (MEDIUM confidence)
- [MDN CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) -- `data-theme` attribute + custom properties pattern
- [Google Fonts](https://fonts.google.com/) -- Font availability for Lora, Inter, Bitter, Source Sans 3, Space Grotesk, Space Mono, DM Sans, Playfair Display, JetBrains Mono

### Tertiary (LOW confidence)
- Theme font pairings for non-Editorial themes -- based on vibe matching, not explicitly specified in project docs

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH -- all native CSS/JS, no libraries needed, patterns directly visible in existing code
- Architecture: HIGH -- reference implementation demonstrates exact carousel pattern; 1080x1080 scale approach specified in CONTEXT.md
- Pitfalls: HIGH -- transform scale issues and font loading are well-documented browser behaviors
- Theme palettes: MEDIUM -- Editorial confirmed from Phase 1 code; other 7 themes have colors from PROJECT.md but font pairings are researcher recommendations

**Research date:** 2026-03-19
**Valid until:** 2026-04-19 (stable -- all native browser features, no library versions to expire)
