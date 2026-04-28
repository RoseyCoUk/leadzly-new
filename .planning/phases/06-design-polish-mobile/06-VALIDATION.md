---
phase: 6
slug: design-polish-mobile
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-28
---

# Phase 6 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Manual browser testing (no automated test framework — static HTML/CSS/JS) |
| **Config file** | none |
| **Quick run command** | Open index.html in browser, resize to 375px |
| **Full suite command** | Manual checklist: hover cards, scroll through all sections, test at 375px |
| **Estimated runtime** | ~3 minutes |

---

## Sampling Rate

- **After every task commit:** Open index.html, verify the specific element changed
- **After every plan wave:** Run full manual checklist
- **Before `/gsd:verify-work`:** Full checklist must pass
- **Max feedback latency:** ~3 minutes

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 6-01-01 | 01 | 1 | DSGN-03 | manual | Open index.html, hover any card — check translateY(-4px) + shadow + green border | ✅ | ⬜ pending |
| 6-01-02 | 01 | 1 | DSGN-03 | manual | Hover `.pricing-card--featured` — confirm `scale(1.03) translateY(-4px)` applies | ✅ | ⬜ pending |
| 6-01-03 | 01 | 1 | DSGN-03 | manual | Open index.html, scroll — sections fade+slide up via IntersectionObserver | ✅ | ⬜ pending |
| 6-01-04 | 01 | 1 | DSGN-04 | manual | Resize to 375px — all grids single column, process arrows hidden, table scrolls horizontally | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

*Existing infrastructure covers all phase requirements. This is a static HTML/CSS/JS project with no test framework — all verification is manual browser testing.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Card lift on hover | DSGN-03 | CSS visual effect — no automation | Hover each card class, confirm translateY(-4px) + elevated box-shadow + green border |
| Section scroll fade-in | DSGN-03 | IntersectionObserver is timing-dependent | Scroll from top, confirm each section smoothly fades+slides up |
| Featured card scale+lift | DSGN-03 | CSS transform stacking | Hover `.pricing-card--featured`, confirm scale and lift both apply |
| Mobile single-column grids | DSGN-04 | Viewport resize required | Set to 375px, check all grid sections |
| Process arrows hidden on mobile | DSGN-04 | Visual check required | 375px, verify `.how__connector` not visible |
| Comparison table horizontal scroll | DSGN-04 | Scroll interaction required | 375px, attempt horizontal scroll on comparison table |
| Floating CTA not obscuring on mobile | DSGN-04 | Visual overlap check | 375px, scroll through page, confirm floating CTA doesn't block key content |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 3 minutes
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
