---
phase: 2
slug: theme-selection-preview
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-03-19
---

# Phase 2 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Browser-based inline smoke tests (inline `<script>` block, same pattern as Phase 1) |
| **Config file** | None — tests are inline in `index.html` |
| **Quick run command** | Open `index.html` in browser, upload test markdown, check console output |
| **Full suite command** | Open `index.html` in browser, upload test markdown, run `runCarouselTests()` in console |
| **Estimated runtime** | ~30 seconds (manual browser check) |

---

## Sampling Rate

- **After every task commit:** Open `index.html`, upload test markdown, visually verify + check console for errors
- **After every plan wave:** Full manual walkthrough of all 8 themes + navigation + console `runCarouselTests()` green
- **Before `/gsd:verify-work`:** Full suite must show no console errors and all 8 themes render correctly

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 2-01-01 | 01 | 0 | THEME-01, THEME-02, THEME-03, PREV-01, PREV-02, PREV-03 | smoke | `runCarouselTests()` in browser console | ❌ W0 | ⬜ pending |
| 2-01-02 | 01 | 1 | THEME-01 | manual + smoke | Visual: 8 swatches visible; console: `document.querySelectorAll('.theme-swatch').length === 8` | ❌ W0 | ⬜ pending |
| 2-01-03 | 01 | 1 | THEME-02 | manual | Click each swatch, verify `data-theme` attribute changes, colors update instantly, no reload | ❌ W0 | ⬜ pending |
| 2-01-04 | 01 | 1 | THEME-03 | manual + smoke | Console: verify 8 distinct `--slide-bg` computed values across themes | ❌ W0 | ⬜ pending |
| 2-01-05 | 01 | 1 | PREV-01 | smoke | Console: `document.querySelectorAll('.slide-frame').length` equals parsed slides count | ❌ W0 | ⬜ pending |
| 2-01-06 | 01 | 1 | PREV-02 | manual + smoke | Click prev/next arrows, click dots, press ← →; console: `goToSlide(1)` changes active class | ❌ W0 | ⬜ pending |
| 2-01-07 | 01 | 1 | PREV-03 | manual | Visual: preview matches expected slide layout; `.slide-frame` has `transform: scale(0.389)` | ❌ W0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] Inline smoke test function `runCarouselTests()` in `index.html` `<script>` block — verifies: 8 swatches exist, correct slide count, `data-theme` attribute present on canvas, `goToSlide` function exists, arrow buttons exist in DOM
- [ ] Test markdown string constant for consistent manual testing (reuse or extend Phase 1 inline test markdown)

*Wave 0 must be committed before Wave 1 tasks begin.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Theme picker pre-selects Editorial on file load | THEME-01 | Visual state, not DOM-checkable without timing | Upload file, verify Editorial swatch has gold border ring before any click |
| Theme colors visually match palette spec | THEME-03 | Subjective color fidelity | Select each theme, compare rendered slide bg/accent colors to palette table in RESEARCH.md |
| Slide transition animation renders smoothly | PREV-01 | Animation quality is visual | Navigate slides, verify opacity/translateX transition is smooth, no flash to full 1080px |
| Preview exactly matches export fidelity | PREV-03 | Requires Phase 3 comparison | Verify `transform: scale(0.389)` on `.slide-frame`; same DOM will be used by Phase 3 html2canvas |
| Left/right arrow disabled states correct | PREV-02 | Visual disabled styling | On slide 1: left arrow visually grayed out, not clickable; on last slide: right arrow grayed out |
| Keyboard navigation works | PREV-02 | Requires user interaction | Press ← and → keys after upload, verify slide changes |
| Dot indicators reflect current slide | PREV-02 | Visual active state | Navigate to each slide, verify corresponding dot is highlighted |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 60s (browser open + upload)
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
