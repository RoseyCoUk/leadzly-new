---
phase: 8
slug: structured-data
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-28
---

# Phase 8 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Manual / browser validation (no test runner — static HTML project) |
| **Config file** | none |
| **Quick run command** | Open index.html in browser, inspect `<head>` for JSON-LD scripts |
| **Full suite command** | schema.org validator + Google Rich Results Test |
| **Estimated runtime** | ~2 minutes per validation pass |

---

## Sampling Rate

- **After every task commit:** Visually inspect `<head>` of index.html for well-formed JSON-LD
- **After every plan wave:** Run full schema.org validator against all three JSON-LD blocks
- **Before `/gsd:verify-work`:** All three blocks must pass Google Rich Results Test with zero errors
- **Max feedback latency:** ~5 minutes

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 8-01-01 | 01 | 1 | SCHEMA-01 | manual | Inspect `<head>` for Organization + LocalBusiness JSON-LD | ✅ index.html | ⬜ pending |
| 8-01-02 | 01 | 1 | SCHEMA-02 | manual | Inspect `<head>` for FAQPage JSON-LD with 7 Q&A pairs | ✅ index.html | ⬜ pending |
| 8-01-03 | 01 | 1 | SCHEMA-03 | manual | Inspect `<head>` for Service JSON-LD (3 services) | ✅ index.html | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

*Existing infrastructure covers all phase requirements — static HTML project requires no test framework installation.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Organization + LocalBusiness valid | SCHEMA-01 | No automated validator in static build | Paste JSON-LD block into schema.org/validator and Google Rich Results Test |
| FAQPage covers all 7 questions | SCHEMA-02 | Content accuracy requires human review | Confirm 7 `@type: Question` entries match page FAQ section |
| Service markup covers 3 services | SCHEMA-03 | Content accuracy requires human review | Confirm telesales, meeting booking, BDM provision entries present |
| Zero validation errors | SCHEMA-01,02,03 | External tool required | Run all three blocks through Google Rich Results Test |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 300s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
