---
phase: 5
slug: booking-experience-ctas
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-28
---

# Phase 5 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Manual browser verification (static HTML/CSS/JS — no test framework) |
| **Config file** | none |
| **Quick run command** | Open `index.html` in browser, check section renders |
| **Full suite command** | Open `index.html` in browser, run through all 4 success criteria |
| **Estimated runtime** | ~30 seconds |

---

## Sampling Rate

- **After every task commit:** Open `index.html` in browser, verify the changed element is visible and correct
- **After every plan wave:** Run full browser check against all phase success criteria
- **Before `/gsd:verify-work`:** Full suite must pass all 4 success criteria
- **Max feedback latency:** 30 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 5-01-01 | 01 | 1 | BOOK-01 | manual | Open index.html → booking section renders two-column grid | ✅ | ⬜ pending |
| 5-01-02 | 01 | 1 | BOOK-01 | manual | Calendly widget loads and is interactive | ✅ | ⬜ pending |
| 5-02-01 | 02 | 1 | BOOK-02 | manual | Floating CTA visible after 3s delay, pulsing dot present | ✅ | ⬜ pending |
| 5-02-02 | 02 | 1 | BOOK-02 | manual | Floating CTA hidden on mobile (<768px) | ✅ | ⬜ pending |
| 5-03-01 | 03 | 2 | BOOK-03 | manual | grep "See If We're a Fit" index.html — FOUND | ✅ | ⬜ pending |
| 5-03-02 | 03 | 2 | BOOK-03 | manual | grep "Get Your Meetings Forecast" index.html — FOUND | ✅ | ⬜ pending |
| 5-03-03 | 03 | 2 | BOOK-03 | manual | grep "Start Filling Your Calendar" index.html — FOUND | ✅ | ⬜ pending |
| 5-04-01 | 04 | 2 | QWIN-03 | manual | Stat counters animate from 0 on first scroll-into-view | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

Existing infrastructure covers all phase requirements. Static HTML/CSS/JS project — no test framework installation needed.

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Calendly widget loads and renders live calendar | BOOK-01 | Requires Calendly account/live CDN; no offline stub | Load index.html in browser → scroll to booking section → confirm Calendly calendar renders |
| Floating CTA 3-second delay | BOOK-02 | Requires real timer execution in browser | Load index.html → wait 3s → confirm floating button appears with pulse dot |
| Floating CTA hidden on mobile | BOOK-02 | Requires real viewport resize | Resize to <768px → confirm floating CTA is not visible |
| Stat counter animation on first scroll | QWIN-03 | Requires real scroll + IntersectionObserver in browser | Load fresh page → scroll hero stats into view → confirm count-up animation plays once |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 30s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
