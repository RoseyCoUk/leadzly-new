# Leadzly Website Overhaul

## What This Is

A comprehensive redesign and content upgrade of the Leadzly landing page (leadzly.co) — a B2B outbound sales agency based in Northern Ireland. The site is a static HTML/CSS page. This project adds trust signals, restructures page flow, adds new conversion sections, and polishes the design — all to close the gap against competitors like Belkins, SalesRoads, and Martal Group.

## Core Value

A prospect visiting leadzly.co should leave with enough trust and urgency to book a strategy call — the page must convert sceptical UK/NI B2B decision-makers.

## Requirements

### Validated

- ✓ Hero section with headline, sub-copy, and CTA — existing
- ✓ Services overview section — existing
- ✓ How it works (4-step process) — existing
- ✓ Pricing section — existing
- ✓ Single testimonial — existing
- ✓ Footer with links — existing
- ✓ Sticky nav with Book a Strategy Call CTA — existing
- ✓ Calendly integration (external link to https://calendly.com/allan-chan-leadzly) — existing

### Active

**Priority 1 — Trust & Social Proof**
- [ ] Expand testimonials to 8–12 with name, title, company, location, star rating, and avatar
- [ ] Expand case studies to 4–5 rich cards (industry tag, timeframe, metrics, story arc, client quote)
- [ ] Trust bar below hero: Clutch rating, "Rated #1 Sales Agency NI", client logo strip, 30-Day Guarantee badge
- [ ] Elevate About section with team info and NI-based differentiator framing

**Priority 2 — Page Structure & Flow**
- [ ] Restructure page order: hero → trust bar → pain points → services → multi-channel → process → industries → case studies → testimonials → comparison table → pricing → FAQ → booking → footer CTA → footer
- [ ] Pain points section ("Does this sound familiar?" — 6 empathy cards)
- [ ] Multi-channel breakdown section (dark navy, 4 channel cards)
- [ ] Industries served grid (12 industry chips with icons)

**Priority 3 — Booking Experience**
- [ ] Inline Calendly embed section (two-column: sell the call + live calendar)
- [ ] Floating sticky CTA button (bottom-right, pulsing green dot, animates in after 3s)
- [ ] Multiple CTA touchpoints with varied copy throughout the page

**Priority 4 — Design Polish**
- [ ] Typography: larger hero h1 (font-weight 900, clamp ≥3.5rem), consistent eyebrow labels above each section
- [ ] Section background alternation (white / light grey / dark navy / green-tinted) for visual rhythm
- [ ] Hover/interaction states: card lift (translateY(-4px)), green border highlight, scroll fade-up animations
- [ ] Mobile responsiveness: all grids → single column, process steps stack, comparison table scrolls, floating CTA hides if overlapping

**Priority 5 — New Sections**
- [ ] Comparison table: Leadzly vs In-House vs Other Agencies (dark navy, 8 rows, green highlight column)
- [ ] FAQ accordion (7 questions, max-width 720px centred, one open at a time)
- [ ] ROI calculator (3 inputs → projected revenue output, inline, no separate page)

**Quick Wins**
- [ ] Meta description and Open Graph tags
- [ ] Favicon
- [ ] Scroll animations (IntersectionObserver fade-up)
- [ ] Counter animation in hero (numbers count up on scroll)
- [ ] Cookie/privacy banner (GDPR compliance)
- [ ] Consistent CTA copy across all touchpoints
- [ ] rel="noopener" on all external links
- [ ] Mobile layout audit and fixes

### Out of Scope

- CMS or backend — static site only, no database or server-side logic
- React, Vue, or any JS framework — vanilla JS only, no build step
- External third-party libraries beyond Calendly embed script — keep dependencies minimal
- Separate pages or subdomains — single-page improvements only
- SEO content writing / blog — copy improvements within existing sections only

## Context

- **Live URL:** leadzly.co
- **Stack:** Pure HTML (`index.html`) + CSS (`style.css`, `base.css`, `page.css`) + vanilla JS (to be added)
- **Brand:** Green `#16a34a`, Dark navy `#0f172a`, Light green `#22c55e`, Green bg `#f0fdf4`; Font: Inter (Google Fonts)
- **Calendly URL:** https://calendly.com/allan-chan-leadzly (used throughout for all CTAs and the inline embed)
- **Logos:** `Leadzly-logo_with-text.svg` and `Leadzly-logo_without-text.svg` already in repo
- **Competitor benchmarks:** Belkins, SalesRoads, Martal Group — research driven by gap analysis in IMPROVEMENTS.md
- **Key differentiator:** NI-based team, no offshore — genuine advantage for UK clients burned by overseas call centres

## Constraints

- **Tech stack:** HTML/CSS/vanilla JS only — no build system, no npm, no frameworks
- **Calendly:** Inline embed uses Calendly widget script (loadable from CDN), no API key needed
- **Fonts:** Inter already loaded via Google Fonts — no new font imports
- **Logo assets:** Must use existing SVG files — no new brand asset creation
- **No backend:** ROI calculator logic must be client-side JS only

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Vanilla JS only | Keeps zero build complexity — site stays as drop-in HTML files | — Pending |
| All 5 priorities in scope | Full improvement plan delivers maximum conversion uplift vs partial | — Pending |
| Inline Calendly embed | External link breaks the booking flow; inline keeps prospect on-page | — Pending |
| Floating sticky CTA | Ensures CTA is always reachable without scrolling to top | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd:transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd:complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-04-24 after initialization*
