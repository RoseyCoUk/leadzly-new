---
phase: 3
slug: trust-social-proof
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-27
---

# Phase 3 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | None — static HTML/CSS, visual verification only |
| **Config file** | None |
| **Quick run command** | Open `index.html` in browser |
| **Full suite command** | Open `index.html` in browser, scroll all sections |
| **Estimated runtime** | ~1 minute (manual scroll-through) |

---

## Sampling Rate

- **After every task commit:** Open `index.html` in browser, verify the changed section renders correctly
- **After every plan wave:** Scroll full page, verify section order and visual treatment
- **Before `/gsd:verify-work`:** Full scroll-through + all 4 TRST requirements visually confirmed

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| logos-copy | 01 | 1 | TRST-05 | Manual | `ls bluepillar-logo.png boltloop_logo.png elevateo_logo.png flooda_logo.png roseyCo_logo.png` | ❌ manual | ⬜ pending |
| trust-bar-html | 01 | 1 | TRST-05 | Manual visual | Open browser → verify trust bar visible between hero and pain points | ❌ manual | ⬜ pending |
| proven-section | 02 | 1 | TRST-03, TRST-04 | Manual visual | Open browser → verify #case-studies shows 4 cards with tags, outcomes, details | ❌ manual | ⬜ pending |
| testimonials-delete | 02 | 1 | D-01 | Manual visual | Open browser → verify no testimonials section present | ❌ manual | ⬜ pending |
| about-reposition | 03 | 2 | TRST-06 | Manual visual | Open browser → verify About section appears between Services and Proven Across Industries | ❌ manual | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

None. This is a static HTML/CSS phase with no test framework to install.

*Existing infrastructure covers all phase requirements.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Trust bar appears below hero, above pain points | TRST-05 | Static HTML — no DOM test harness | Open index.html, scroll to hero bottom, verify trust bar band with logos + badges |
| Trust bar logos display greyscale at ~0.6 opacity | TRST-05 | Visual CSS rendering | Inspect logos in DevTools, verify `filter: grayscale(100%)` and `opacity: 0.6` |
| 4 industry cards visible with correct tag/outcome/detail | TRST-03, TRST-04 | Static HTML content | Scroll to #case-studies, count 4 cards, verify tag chip (green), outcome text, detail text per card |
| `id="case-studies"` preserved on proven section | Nav integrity | Static HTML attribute | Click "Our Work" or "Results" nav link, verify browser jumps to proven section |
| About section positioned between Services and Proven | TRST-06 | Section order is visual | Scroll from Services → confirm About appears next → confirm Proven appears after |
| No testimonials section visible | D-01 | Deleted HTML block | Scroll full page, confirm no `.testimonials` section exists |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < ~60s (manual browser check)
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
