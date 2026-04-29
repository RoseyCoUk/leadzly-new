---
phase: 9
slug: performance-core-web-vitals
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-28
---

# Phase 9 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | none — static HTML/CSS site, manual + grep verification only |
| **Config file** | none |
| **Quick run command** | `grep -c "" index.html page.css` (sanity check files exist) |
| **Full suite command** | manual Lighthouse audit in Chrome DevTools |
| **Estimated runtime** | ~5 minutes (manual) |

---

## Sampling Rate

- **After every task commit:** Run grep checks per acceptance criteria
- **After every plan wave:** Visual check in browser + network tab inspection
- **Before `/gsd:verify-work`:** Lighthouse Performance audit must score 90+
- **Max feedback latency:** immediate (grep checks) / ~5 min (Lighthouse)

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 9-01-01 | 01 | 1 | PERF-01 | grep | `grep "Inter:wght@400;600;700" index.html` | ✅ | ⬜ pending |
| 9-01-02 | 01 | 1 | PERF-02 | grep | `grep 'loading="lazy"' index.html` | ✅ | ⬜ pending |
| 9-01-03 | 01 | 1 | PERF-03 | grep | `grep "min-height" page.css` | ✅ | ⬜ pending |
| 9-01-04 | 01 | 1 | PERF-04 | grep | `grep -r "floating-cta\|fab-cta" index.html page.css` | ✅ | ⬜ pending |
| 9-02-01 | 02 | 2 | PERF-05 | grep | `grep "page.css?v=" index.html` | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

*Existing infrastructure covers all phase requirements — static site with no test framework.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Lighthouse score ≥ 90 | PERF-01–05 combined | No automated runner | Open Chrome DevTools → Lighthouse → Performance → Generate report |
| CLS = 0 from Calendly | PERF-03 | Requires live render | Open page, observe layout shift in Performance tab during Calendly load |
| SVGs don't appear in initial render waterfall | PERF-02 | Network tab inspection | Open Network tab, reload, confirm SVG files load after initial paint |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 60s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
