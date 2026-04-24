# Project State

## Project Reference
See: .planning/PROJECT.md

**Core value:** A prospect visiting leadzly.co should leave with enough trust and urgency to book a strategy call — the page must convert sceptical UK/NI B2B decision-makers.
**Current focus:** Phase 1 — Foundation & Hygiene

---

## Current Status

**Phase:** 1 — Foundation & Hygiene
**Status:** Not started
**Last updated:** 2026-04-24

**Progress bar:**
```
Phase 1 [----------] 0%
Phase 2 [----------] 0%
Phase 3 [----------] 0%
Phase 4 [----------] 0%
Phase 5 [----------] 0%
Phase 6 [----------] 0%
```

---

## Phase Log

| Phase | Name | Status | Completed |
|-------|------|--------|-----------|
| 1 | Foundation & Hygiene | Not started | — |
| 2 | Page Restructure & Scaffolding | Not started | — |
| 3 | Trust & Social Proof | Not started | — |
| 4 | Conversion Sections | Not started | — |
| 5 | Booking Experience & CTAs | Not started | — |
| 6 | Design Polish & Mobile | Not started | — |

---

## Performance Metrics

| Metric | Baseline | Target | Current |
|--------|----------|--------|---------|
| Booking CTA clicks | Unknown | +50% | — |
| Page sections count | ~6 | 15 | — |
| Testimonials visible | 1 | 8–12 | — |
| Case studies visible | 0 | 4–5 | — |
| Lighthouse Performance | Unknown | 90+ | — |

---

## Accumulated Context

### Key Decisions
- Vanilla JS only — no build system, no npm, no frameworks (locked in PROJECT.md)
- Inline Calendly embed via widget script loaded from Calendly CDN
- All ROI calculator logic is client-side JS, no server call
- Floating sticky CTA hides or repositions on mobile to avoid overlap
- Scroll animations use IntersectionObserver (no library)
- Counter animation in hero uses IntersectionObserver + requestAnimationFrame

### Technical Notes
- Stack: `index.html` + `style.css`, `base.css`, `page.css` + vanilla JS (new file to be added)
- Brand colours: Green `#16a34a`, Dark navy `#0f172a`, Light green `#22c55e`, Green bg `#f0fdf4`
- Font: Inter (already loaded via Google Fonts — no new imports)
- Logos: `Leadzly-logo_with-text.svg` and `Leadzly-logo_without-text.svg` already in repo
- Calendly URL: `https://calendly.com/allan-chan-leadzly` — used for all CTAs and inline embed
- GA4 already installed via Partytown (do not re-add analytics)

### Todos
- [ ] Source or create placeholder testimonial content (8–12 entries) before Phase 3
- [ ] Source or create placeholder case study content (4–5 entries) before Phase 3
- [ ] Confirm favicon asset exists or create one before Phase 1

### Blockers
- None currently

---

## Session Continuity

**Next action:** Run `/gsd:plan-phase 1` to plan Phase 1 — Foundation & Hygiene

**Context for next session:**
Phase 1 covers NAV-01 (sticky nav with blur/shadow), NAV-02 (hamburger menu), NAV-03 (rel=noopener on external links), QWIN-01 (meta + OG tags), QWIN-02 (favicon), and QWIN-04 (GDPR cookie banner). These are all low-risk, self-contained changes to `index.html` and CSS — safe to tackle before any structural HTML reordering.

---
*State initialised: 2026-04-24*
*Last updated: 2026-04-24 after roadmap creation*
