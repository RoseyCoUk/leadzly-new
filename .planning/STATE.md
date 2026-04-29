---
gsd_state_version: 1.0
milestone: v1.1
milestone_name: SEO & Performance
current_plan: 2
status: executing
last_updated: "2026-04-29T01:47:40.202Z"
last_activity: 2026-04-29
progress:
  total_phases: 3
  completed_phases: 2
  total_plans: 6
  completed_plans: 5
---

# Project State

## Project Reference

See: .planning/PROJECT.md (updated 2026-04-28)

**Core value:** A prospect visiting leadzly.co should leave with enough trust and urgency to book a strategy call — the page must convert sceptical UK/NI B2B decision-makers.
**Current focus:** Phase 09 — performance-core-web-vitals

---

## Current Status

**Phase:** 9
**Current Plan:** 1
**Status:** Executing Phase 09
**Last updated:** 2026-04-29

**Progress bar:**

```
v1.1 Phase 7 [██████████] 100%  Meta Hygiene
v1.1 Phase 8 [██████████] 100%  Structured Data (2/2 plans complete)
v1.1 Phase 9 [████████░░]  83%  Performance & Core Web Vitals (1/2 plans complete)
```

---

## Phase Log

| Phase | Name | Status | Completed |
|-------|------|--------|-----------|
| 7 | Meta Hygiene | Complete | 2026-04-28 |
| 8 | Structured Data | Complete | 2026-04-29 |
| 9 | Performance & Core Web Vitals | In progress | — |

---

## Performance Metrics

| Metric | Baseline | Target | Current |
|--------|----------|--------|---------|
| Lighthouse Performance | Unknown | 90+ | — |
| CLS score | Unknown | 0 (controllable sources) | — |
| Structured data errors | Unknown | 0 | — |
| Google Fonts weight variants | ~8 | 3 (400/600/700) | — |

---
| Phase 07-meta-hygiene P01 | 5 | 2 tasks | 1 files |
| Phase 07-meta-hygiene P02 | 5 | 2 tasks | 2 files |
| Phase 08-structured-data P01 | 5 | 1 tasks | 1 files |
| Phase 08-structured-data P02 | 8 | 2 tasks | 1 files |
| Phase 08-structured-data P02 | 10 | 2 tasks | 1 files |
| Phase 09-performance-core-web-vitals P01 | 15 | 3 tasks | 4 files |

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
- No will-change added to card hover rules — avoids GPU layer overhead on mobile [06-01]
- .about__diff-item requires border: 1px solid transparent baseline for border-color hover transition to be visible [06-01]
- .pricing-card--featured:hover on desktop uses scale(1.03) translateY(-4px) compound transform to preserve existing desktop scale [06-01]
- Hero and trust-bar sections excluded from fade-in — above fold at load, JS guard skips them regardless, no visual gain from adding [06-02]
- 13 below-fold sections tagged with fade-in in a single batch commit — JS IntersectionObserver stagger (60ms) produces rapid cascade, not simultaneous appearance [06-02]
- Twitter card type summary chosen (not summary_large_image) — logo is square, summary_large_image expects 2:1 landscape ratio [07-01]
- twitter:site omitted from Twitter Card tags — Leadzly X handle unconfirmed, optional tag excluded per plan [07-01]
- Canonical href uses trailing slash https://leadzly.co/ — consistent with og:url and upcoming sitemap.xml [07-01]
- Allow: / without Disallow in robots.txt — fully public site, no pages to block [07-02]
- sitemap.xml uses trailing slash on loc URL — matches canonical href from 07-01 to avoid Google URL mismatch [07-02]
- contactPoint in SCHEMA-01 uses Calendly URL not email/phone — no visible contact info on page, schema-content mismatch avoided [08-01]
- areaServed in SCHEMA-01: United Kingdom + Northern Ireland only — Republic of Ireland excluded (not claimed on page) [08-01]
- JSON-LD blocks: separate <script type="application/ld+json"> per schema type (not combined @graph) — easier to validate individually [08-01]
- SCHEMA-01 address uses only addressRegion/addressCountry — no street address on page, fabrication avoided [08-01]
- FAQPage question name fields match visible FAQ button span text verbatim after entity decoding — HTML entities decoded to Unicode before JSON insertion [08-02]
- Service JSON-LD uses @graph array grouping 4 Service nodes (Telesales Outsourcing, Meeting Booking, BDM Provision, Sales Enablement) — all 4 visible services included [08-02]
- Service description values match .service-card__desc text exactly — em-dashes are Unicode, not HTML entities [08-02]
- All JSON-LD text values are plain Unicode — no HTML entities permitted in JSON string values (pattern established across all three blocks) [08-02]
- Font weight 500 upgraded to 600 for all Inter and PJS rules — nearest loaded weight above 500, avoids synthetic bold fallback [09-01]
- SVG mockups externalised as standalone files loaded via lazy img tags — reduces HTML parse payload by ~320 lines [09-01]
- Calendly container uses min-height not height — allows iframe expansion if widget renders taller content [09-01]

### Technical Notes

- Stack: `index.html` + `style.css`, `base.css`, `page.css` + vanilla JS (inline IIFEs, no separate file)
- Brand colours: Green `#16a34a`, Dark navy `#0f172a`, Light green `#22c55e`, Green bg `#f0fdf4`
- Font: Inter (already loaded via Google Fonts — no new imports)
- Logos: `Leadzly-logo_with-text.svg` and `Leadzly-logo_without-text.svg` already in repo
- Calendly URL: `https://calendly.com/allan-chan-leadzly` — used for all CTAs and inline embed
- GA4 already installed via Partytown (do not re-add analytics)
- v1.1 note: JSON-LD blocks go in `<head>` before closing `</head>` tag
- v1.1 note: robots.txt and sitemap.xml are new files at project root (served by Vercel as static assets)
- v1.1 note: page.css minification is hand-applied — no build tool; version bump via query string on link tag in index.html

### Todos

- [x] Confirm exact FAQ question/answer text before writing SCHEMA-02 — resolved: all 7 Q&A pairs extracted verbatim in 08-RESEARCH.md [08-01]
- [x] Confirm OG image URL for Twitter Card meta tags (twitter:image must be absolute URL) — resolved: https://leadzly.co/Leadzly-logo_with-text.png [07-01]

### Blockers

- None currently

---

### Quick Tasks Completed

| # | Description | Date | Commit | Directory |
|---|-------------|------|--------|-----------|
| 260428-hbj | Add graphics to the site to bring it to life | 2026-04-28 | 9126d98 | [260428-hbj-add-graphics-to-the-site-to-bring-it-to-](./quick/260428-hbj-add-graphics-to-the-site-to-bring-it-to-/) |

---

## Session Continuity

**Next action:** Execute Phase 09 Plan 02 — Floating CTA CLS fix (PERF-04) and CSS minification (PERF-05).
Last activity: 2026-04-29

**Context for next session:**
Phase 09 Plan 01 complete: Google Fonts trimmed to 7 variants (PJS 400/600/700/800, Inter 400/600/700), weight 500 removed from 5 CSS rules (all now 600). Both inline SVGs externalised — outreach-sequence.svg (480x520) and pipeline-board.svg (640x500) — as lazy-loaded img tags with explicit width/height. CSS selectors updated from svg to img. Calendly container gets min-height: 700px desktop / 650px mobile. PERF-01, PERF-02, PERF-03 complete. Remaining: PERF-04 (floating CTA CLS fix) and PERF-05 (CSS minification).

---
*State initialised: 2026-04-24*
*Last session: 2026-04-29 — Phase 08 Plan 02 complete (SCHEMA-02, SCHEMA-03) — Phase 08 done*
