---
phase: 2
slug: page-restructure-scaffolding
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-24
---

# Phase 2 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Manual browser verification (static HTML/CSS — no test framework) |
| **Config file** | none |
| **Quick run command** | Open index.html in browser, scroll top-to-bottom |
| **Full suite command** | Open index.html, verify all 5 success criteria from roadmap |
| **Estimated runtime** | ~2 minutes |

---

## Sampling Rate

- **After every task commit:** Open index.html in browser and verify the changed section renders correctly
- **After every plan wave:** Full scroll-through verifying section order and backgrounds
- **Before `/gsd:verify-work`:** Full suite must be green
- **Max feedback latency:** ~120 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 2-01-01 | 01 | 1 | PAGE-01 | manual | Open browser, scroll top-to-bottom, verify 15-section order | ✅ | ⬜ pending |
| 2-01-02 | 01 | 1 | PAGE-02 | manual | Verify pain points section with 6 cards visible | ✅ | ⬜ pending |
| 2-01-03 | 01 | 1 | PAGE-03 | manual | Verify dark navy multi-channel section with 4 channel cards | ✅ | ⬜ pending |
| 2-01-04 | 01 | 1 | PAGE-04 | manual | Verify 12-chip industry grid visible | ✅ | ⬜ pending |
| 2-02-01 | 02 | 2 | DSGN-02 | manual | Scroll page and verify background alternation pattern | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

*Existing infrastructure covers all phase requirements. Static HTML/CSS requires no test framework installation.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| 15-section scroll order | PAGE-01 | DOM structure only verifiable visually | Open index.html, scroll from top to bottom, confirm exact section order |
| Pain points 6-card grid | PAGE-02 | Layout/render requires browser | Find pain-points section, count cards, check empathetic copy |
| Dark navy multi-channel (4 cards) | PAGE-03 | Colour/contrast requires visual check | Find multi-channel section, verify dark navy bg, 4 channel cards with white text |
| Industries 12-chip grid | PAGE-04 | Icon + label layout requires browser | Find industries section, count chips, verify icon + label format |
| Background alternation | DSGN-02 | Colour rendering requires visual check | Scroll slowly, confirm white→grey→navy→green-tinted alternation |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 120s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
