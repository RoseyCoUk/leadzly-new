---
phase: 12
slug: aeo-copy-optimisation
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-29
---

# Phase 12 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Manual browser review — no automated test framework on this static HTML project |
| **Config file** | none |
| **Quick run command** | `open index.html in browser, expand each FAQ, read first sentence` |
| **Full suite command** | `open index.html in browser, scroll all sections, view-source to verify JSON-LD` |
| **Estimated runtime** | ~2 minutes |

---

## Sampling Rate

- **After every task commit:** Visual review of changed section in browser
- **After every plan wave:** Full page scroll-through, verify all modified sections
- **Before `/gsd:verify-work`:** Full review must pass — all 7 FAQ answers, hero, About, JSON-LD
- **Max feedback latency:** ~120 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 12-01-01 | 01 | 1 | AEO-01 | manual | Open index.html, expand FAQ Q1 — first sentence must be self-contained | N/A | ⬜ pending |
| 12-01-02 | 01 | 1 | AEO-01 | manual | Expand FAQ Q2–Q7 — each first sentence self-contained without reading question | N/A | ⬜ pending |
| 12-01-03 | 01 | 1 | AEO-01 | manual | View-source → search FAQPage JSON-LD → confirm `text` values match visible HTML, no HTML entities | N/A | ⬜ pending |
| 12-02-01 | 02 | 1 | AEO-02 | manual | View hero section — count numerical stats in sub-headline, must be ≥2 | N/A | ⬜ pending |
| 12-02-02 | 02 | 1 | AEO-03 | manual | Read About paragraph — flag any vague claim ("experienced", "growth is our growth"); all claims must be verifiable from page content | N/A | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

None — existing infrastructure covers all phase requirements. This is pure copy editing on a static HTML file. No test files need to be created.

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| FAQ first sentence self-contained | AEO-01 | Static HTML, no automated test framework | Open index.html in browser. Expand each of 7 FAQ items. Cover the question. Read only the first sentence of the answer. Does it make sense standalone? |
| JSON-LD FAQPage text matches visible HTML | AEO-01 | Schema is in-page JSON, no validation tooling configured | View-source on index.html. Search for `FAQPage`. Compare each `"text"` value to the visible answer. Check for HTML entities (none allowed in JSON strings). |
| Hero sub-headline has ≥2 verifiable stats | AEO-02 | Content quality judgment | View hero section. Count items with specific numerical values (e.g. "20+", "12 sectors"). Must find at least 2. |
| About paragraph has no vague agency language | AEO-03 | Content quality judgment | Read About section. Flag phrases like "experienced", "track record", "your growth is our growth". All claims must be specific and supported by existing page content. |

---

## Validation Sign-Off

- [ ] All tasks have manual review verification steps
- [ ] Sampling continuity: each plan's tasks reviewed before moving to next plan
- [ ] Wave 0: N/A — no test infrastructure needed
- [ ] No watch-mode flags
- [ ] Feedback latency < 120s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
