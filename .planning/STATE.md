---
gsd_state_version: 1.0
milestone: v1.0
milestone_name: milestone
current_plan: 1
status: executing
last_updated: "2026-04-28T15:40:06.570Z"
progress:
  total_phases: 6
  completed_phases: 4
  total_plans: 13
  completed_plans: 15
---

# Project State

## Project Reference

See: .planning/PROJECT.md

**Core value:** A prospect visiting leadzly.co should leave with enough trust and urgency to book a strategy call — the page must convert sceptical UK/NI B2B decision-makers.
**Current focus:** Phase 05 — booking-experience-ctas

---

## Current Status

**Phase:** 5
**Current Plan:** 1
**Status:** Executing Phase 05
**Last updated:** 2026-04-28

**Progress bar:**

```
Phase 1 [----------] 0%
Phase 2 [██████████] 100%
Phase 3 [██████████] 100%
Phase 4 [██████████] 100%
Phase 5 [███-------] 33%
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
| Phase 02-page-restructure-scaffolding P01 | 12 | 1 tasks | 1 files |
| Phase 03-trust-social-proof P01 | 12 | 2 tasks | 7 files |
| Phase 03-trust-social-proof P02 | 12 | 2 tasks | 2 files |
| Phase 04-conversion-sections P01 | 5 | 2 tasks | 1 files |
| Phase 04-conversion-sections P02 | 8 | 2 tasks | 2 files |
| Phase 04-conversion-sections P03 | 8 | 2 tasks | 2 files |
| Phase 04-conversion-sections P04 | 30 | 3 tasks | 2 files |
| Phase 05-booking-experience-ctas P01 | 73 | 2 tasks | 2 files |
| Phase 05-booking-experience-ctas P02 | 5 | 2 tasks | 2 files |
| Phase 05-booking-experience-ctas P03 | 5 | 2 tasks | 2 files |
| Phase 05-booking-experience-ctas P04 | 1 | 1 tasks | 1 files |

## Accumulated Context

### Key Decisions

- Vanilla JS only — no build system, no npm, no frameworks (locked in PROJECT.md)
- Inline Calendly embed via widget script loaded from Calendly CDN
- All ROI calculator logic is client-side JS, no server call
- Floating sticky CTA hides or repositions on mobile to avoid overlap
- Scroll animations use IntersectionObserver (no library)
- Counter animation in hero uses IntersectionObserver + requestAnimationFrame
- About section placed between pricing and faq with Phase 3 relocation comment (TRST-06 will move it) [02-01]
- Hero "See Our Results" CTA updated from #results to #case-studies alongside nav and footer links [02-01]
- Duplicate CSS selectors resolved by merging Task 1 background-only rules into Task 2 full rules — one authoritative block per selector [02-02]
- industry-chip uses explicit background:#ffffff (not --color-primary-highlight) for contrast on green-tinted section bg [02-02]
- Five partner logos rendered greyscale at 0.6 opacity with hover colour reveal — signals real clients without visual noise [03-01]
- Trust bar uses var(--section-tint) background — no hardcoded hex, maintains design token discipline [03-01]
- Divider hidden at <640px to allow graceful mobile wrap of logos and badges [03-01]
- Proven Across Industries uses aggregate stat "100+ meetings booked monthly" — honest portfolio-level claim, no fabricated per-client metrics [03-02]
- id="case-studies" preserved on .proven section so all nav and footer anchor links continue to work [03-02]
- Testimonials scaffold section deleted entirely — no empty sections remain on page [03-02]
- fade-in class added to all 4 proven cards as Phase 6 animation hook — zero cost now, saves rework later [03-02]
- About section moved from post-Pricing to between Services and Multi-Channel — credibility narrative now precedes evidence sections (TRST-06) [03-03]
- id="about" preserved after repositioning — all nav and footer anchor links remain functional [03-03]
- Hero h1 clamp maximum also raised from 4.25rem to 4.75rem alongside the minimum — gives more visual headroom at large desktop viewports [04-01]
- Single .section-label rule confirmed in page.css — no overrides in style.css or base.css, single edit is authoritative [04-01]
- comparison__cell--leadzly uses display: table-cell to ensure consistent cell rendering alongside other td elements [04-02]
- color-primary-highlight used for Leadzly thead cell — green tint visually separates the column header at a glance [04-02]
- .comparison section-title/subtitle/label have explicit colour overrides — dark background requires white text for readability [04-02]
- FAQ IIFE added as separate script block — isolates concern from existing nav/cookie script block [04-03]
- max-height: 400px used for FAQ answer CSS transition target (not auto) — required for CSS transition to animate; sufficient for all answer lengths [04-03]
- Comparison table reverted from dark navy to var(--color-primary-soft) light green — consistent with page light theme after human visual review [04-04]
- FAQ answer transition upgraded to 0.4s ease with 600px ceiling; font-size reduced to 0.95rem; padding-left 1.5rem added [04-04]
- ROI inputs given explicit height: 48px for cross-browser rendering parity across all three number inputs [04-04]
- Calendly CDN loaded once in <head> — widget.css link + widget.js async script [05-01]
- Booking grid uses 1fr/1.2fr split — calendar column slightly wider to accommodate embed [05-01]
- Calendly height 700px desktop / 650px mobile — eliminates internal scrollbar in widget [05-01]
- Floating CTA reveal uses CSS transition on --visible modifier class (opacity + translateY) toggled by JS IIFE setTimeout 3000ms [05-02]
- Floating CTA hidden via display none at max-width 639px — no overlap on mobile [05-02]
- Hero secondary CTA changed from #services to #booking — reduces friction by directing mid-page visitors straight to calendar embed [05-03]
- All three pricing card CTAs unified as 'Start Filling Your Calendar' — consistent intent signal at the pricing decision point [05-03]
- ROI CTA uses .roi__cta wrapper div with CSS token margin — layout isolation, no inline styles [05-03]
- Hero stat counter uses animated flag + observer.disconnect() to play exactly once per page load — no replay on re-scroll [05-04]
- ease-out cubic (1 - Math.pow(1-progress,3)) chosen for natural deceleration — data feels live and real [05-04]
- data-count-to / data-count-suffix attributes keep animation targets declarative in HTML — no hardcoded values in scripts [05-04]

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

**Next action:** Human verify checkpoint — open index.html in browser and verify all Phase 5 deliverables (BOOK-01, BOOK-02, BOOK-03, QWIN-03). On approval, advance to Phase 6.

**Context for next session:**
Phase 5 Plan 04 implementation complete — hero stat counter IIFE added: numbers count up 0→target over 1200ms ease-out cubic on first scroll-into-view. Awaiting human verify checkpoint for full Phase 5 sign-off (4 plans: Calendly embed, floating CTA, CTA copy variants, stat counter animation).

---
*State initialised: 2026-04-24*
*Last session: 2026-04-28 — Completed: 05-booking-experience-ctas 05-04-PLAN.md (awaiting human-verify checkpoint)*
