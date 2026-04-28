---
phase: 4
slug: conversion-sections
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-27
---

# Phase 4 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | none — vanilla HTML/CSS/JS; manual browser inspection + DOM checks |
| **Config file** | none |
| **Quick run command** | Open `index.html` in browser, inspect DOM |
| **Full suite command** | Visual review of all 4 requirements (SECT-01, SECT-02, SECT-03, DSGN-01) |
| **Estimated runtime** | ~2 minutes manual |

---

## Sampling Rate

- **After every task commit:** Verify the specific element added/modified exists in `index.html` via grep
- **After every plan wave:** Visual browser check of affected sections
- **Before `/gsd:verify-work`:** Full suite must be green
- **Max feedback latency:** 60 seconds (grep checks are instant)

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | Status |
|---------|------|------|-------------|-----------|-------------------|--------|
| 04-01-01 | 01 | 1 | DSGN-01 | grep | `grep -n "clamp(3.5rem" page.css` | ⬜ pending |
| 04-01-02 | 01 | 1 | DSGN-01 | grep | `grep -n "font-weight: 700" page.css` (section-label) | ⬜ pending |
| 04-02-01 | 02 | 2 | SECT-01 | grep | `grep -n "comparison__table" index.html` | ⬜ pending |
| 04-02-02 | 02 | 2 | SECT-01 | grep | `grep -n "color-dark" page.css` (comparison background) | ⬜ pending |
| 04-03-01 | 03 | 3 | SECT-02 | grep | `grep -n "faq__item" index.html` | ⬜ pending |
| 04-03-02 | 03 | 3 | SECT-02 | grep | `grep -n "faq__answer" page.css` | ⬜ pending |
| 04-04-01 | 04 | 4 | SECT-03 | grep | `grep -n "roi" index.html` | ⬜ pending |
| 04-04-02 | 04 | 4 | SECT-03 | grep | `grep -n "roi__output" page.css` | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

None — no test framework required. Verification is grep-based (instant) plus manual browser visual check.

*Existing infrastructure covers all phase requirements.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| FAQ accordion opens one item, collapses others | SECT-02 | JS interaction — cannot grep | Click each FAQ item; verify only the clicked item expands |
| ROI calculator computes correct output | SECT-03 | Math correctness in browser — cannot grep | Enter deal value 5000, current 4, target 20; verify output = £8,000/month and £96,000/year |
| Comparison Leadzly column highlighted green | SECT-01 | Visual rendering — cannot grep | Scroll to comparison table; verify Leadzly column has green background against dark navy |
| Hero h1 renders ≥3.5rem at desktop | DSGN-01 | Visual/devtools check | Open DevTools → inspect h1 → computed font-size ≥ 56px at 1200px viewport |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 60s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
