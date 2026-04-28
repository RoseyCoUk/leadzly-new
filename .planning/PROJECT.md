# Leadzly Website Overhaul

## Current Milestone: v1.1 SEO & Performance

**Goal:** Make leadzly.co crawlable, indexable, and fast enough to rank — structured data, meta hygiene, and Core Web Vitals fixes.

**Target features:**
- Meta hygiene: canonical, robots tag, Twitter Cards, sitemap.xml, robots.txt
- Structured data (JSON-LD): Organization/LocalBusiness, FAQPage, Service schemas
- Performance: font weight trim, SVG lazy-loading, CLS fixes (Calendly + floating CTA), CSS minification

---

## What This Is

A fully redesigned and conversion-optimised landing page for Leadzly (leadzly.co) — a B2B outbound sales agency based in Northern Ireland. The static HTML/CSS/JS page was rebuilt from a bare-bones site into a full-funnel conversion page with trust signals, a 14-section flow, inline booking, interactive tools (ROI calculator, FAQ, comparison table), scroll animations, and dashboard-style SVG illustrations.

**v1.0 shipped: 2026-04-28 — live on Vercel via RoseyCoUk/leadzly-new**

## Core Value

A prospect visiting leadzly.co should leave with enough trust and urgency to book a strategy call — the page must convert sceptical UK/NI B2B decision-makers.

## Requirements

### Validated (v1.0)

- ✓ Sticky nav with blur/shadow on scroll (NAV-01) — v1.0 (confirmed already live)
- ✓ Hamburger menu on mobile (NAV-02) — v1.0 (confirmed already live)
- ✓ All external links `rel="noopener noreferrer"` (NAV-03) — v1.0 (confirmed already live)
- ✓ OG tags (title, description, image, URL) and favicon (QWIN-01, QWIN-02) — v1.0
- ✓ GDPR cookie consent banner with GA4 gating (QWIN-04) — v1.0
- ✓ 14-section page structure in correct order (PAGE-01) — v1.0
- ✓ Pain points section — 6 empathy cards (PAGE-02) — v1.0
- ✓ Multi-channel breakdown — 4 channel cards, dark navy (PAGE-03) — v1.0
- ✓ Industries served — 12-chip grid with icons (PAGE-04) — v1.0
- ✓ Section background alternation — white/grey/navy/green-tinted (DSGN-02) — v1.0
- ✓ Trust bar below hero — partner logos, stat badge, 30-day guarantee (TRST-05) — v1.0
- ✓ Proven Across Industries — 4 industry outcome cards (TRST-03, TRST-04) — v1.0
- ✓ About section repositioned above multi-channel, NI-first framing (TRST-06) — v1.0
- ✓ Hero h1 font-weight 800, clamp ≥3.5rem; green eyebrow labels on all sections (DSGN-01) — v1.0
- ✓ Comparison table — Leadzly vs In-House vs Other Agencies, 8 rows (SECT-01) — v1.0
- ✓ FAQ accordion — 7 questions, one open at a time (SECT-02) — v1.0
- ✓ ROI calculator — 3 inputs, client-side JS, projected revenue output (SECT-03) — v1.0
- ✓ Inline Calendly embed — two-column layout, sell + calendar (BOOK-01) — v1.0
- ✓ Floating sticky CTA — pulsing dot, 3s delay, hidden on mobile (BOOK-02) — v1.0
- ✓ Varied CTA copy across all major sections (BOOK-03) — v1.0
- ✓ Counter animation in hero — numbers count up on first scroll (QWIN-03) — v1.0
- ✓ Card hover lift + green border + scroll fade-up animations (DSGN-03) — v1.0
- ✓ Full mobile responsiveness — single-column grids, horizontal table scroll (DSGN-04) — v1.0
- ✓ Dashboard-style SVG illustrations — How It Works CRM board, Multi-Channel sequence UI (quick task) — v1.0

### Dropped (intentional)

- ✗ TRST-01 — 8–12 testimonial cards — *dropped: no real testimonial content available; testimonials section deleted entirely to avoid fake social proof*
- ✗ TRST-02 — Testimonials 3-column grid — *dropped: same reason as TRST-01*

### Active (v1.1)

- [x] META-01: Canonical URL tag — Validated in Phase 07: meta-hygiene
- [x] META-02: Meta robots tag — Validated in Phase 07: meta-hygiene
- [x] META-03: Twitter Card meta tags — Validated in Phase 07: meta-hygiene
- [x] META-04: robots.txt — Validated in Phase 07: meta-hygiene
- [x] META-05: sitemap.xml — Validated in Phase 07: meta-hygiene
- [ ] SCHEMA-01: Organization + LocalBusiness JSON-LD
- [ ] SCHEMA-02: FAQPage JSON-LD
- [ ] SCHEMA-03: Service JSON-LD
- [ ] PERF-01: Font weight trim (400/600/700 only)
- [ ] PERF-02: SVG lazy-loading (pipeline + outreach mockups)
- [ ] PERF-03: Calendly CLS fix
- [ ] PERF-04: Floating CTA CLS fix
- [ ] PERF-05: page.css minification

### Future (v1.2 candidates)

- [ ] Testimonials section — when real client testimonials are sourced (TRST-01/02 revival)
- [ ] Replace stock hero image with NI/UK-specific B2B photography (V2-05)
- [ ] Team photos in About section (V2-04, blocked on asset sourcing)
- [ ] Live Clutch review widget if API becomes available (V2-01)
- [ ] Active section highlighting in nav on scroll (V2-02)
- [ ] Phone number in nav (V2-03)

### Out of Scope

- CMS or backend — static site only, no database or server-side logic
- React, Vue, or any JS framework — vanilla JS only, no build step
- External third-party libraries beyond Calendly embed script — keep dependencies minimal
- Separate pages or subdomains — single-page improvements only
- SEO content writing / blog — copy improvements within existing sections only

## Context

- **Live URL:** leadzly.co (deployed via Vercel — RoseyCoUk/leadzly-new)
- **Stack:** `index.html` + `style.css`, `base.css`, `page.css` + vanilla JS (inline IIFEs, no separate file)
- **Size:** ~3,350 LOC across HTML + CSS files
- **Brand:** Green `#16a34a`, Dark navy `#0f172a`, Light green `#22c55e`, Green bg `#f0fdf4`; Font: Inter (Google Fonts)
- **Calendly URL:** https://calendly.com/allan-chan-leadzly (used throughout for all CTAs and the inline embed)
- **Logos:** `Leadzly-logo_with-text.svg` and `Leadzly-logo_without-text.svg` in project root
- **Competitor benchmarks:** Belkins, SalesRoads, Martal Group
- **Key differentiator:** NI-based team, no offshore — genuine advantage for UK clients burned by overseas call centres
- **GA4:** Installed via Partytown, gated behind GDPR consent banner

## Constraints

- **Tech stack:** HTML/CSS/vanilla JS only — no build system, no npm, no frameworks
- **Calendly:** Inline embed uses Calendly widget script (loadable from CDN), no API key needed
- **Fonts:** Inter already loaded via Google Fonts — no new font imports
- **Logo assets:** Must use existing SVG files — no new brand asset creation
- **No backend:** ROI calculator logic must be client-side JS only

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Vanilla JS only | Keeps zero build complexity — site stays as drop-in HTML files | ✓ Good |
| Inline Calendly embed | External link breaks the booking flow; inline keeps prospect on-page | ✓ Good |
| Floating sticky CTA | Ensures CTA is always reachable without scrolling to top | ✓ Good |
| Testimonials deleted entirely | No real content; fake social proof undermines credibility with sceptical B2B buyers | ✓ Good |
| No dark mode | Added complexity without payoff on a focused B2B landing page | ✓ Good |
| Light-only palette: white / #f8fafb / #f0fdf4 | Three tones only — clean, professional, consistent with competitors | ✓ Good |
| Green solid nav (#16a34a) | Immediately anchors brand colour; white nav was too plain against hero | ✓ Good |
| Multi-channel section: light not dark | Dark mid-page sections broke clean light palette | ✓ Good |
| Comparison table: light green not dark navy | Reverted after human visual review — consistent with page light theme | ✓ Good |
| Hero: split layout + meeting card SVG | No real photos; meeting card communicates value prop visually | ✓ Good |
| About section between Services and Multi-Channel | Credibility narrative precedes evidence sections | ✓ Good |
| Calendly height 700px desktop / 650px mobile | Eliminates internal scrollbar in widget | ✓ Good |
| Counter animation — play once per load | observer.disconnect() after first trigger; no replay on re-scroll | ✓ Good |
| Dashboard SVG illustrations inline | No external images; scales with viewport; matches brand palette | ✓ Good |
| Inline IIFEs, no separate JS file | Keeps deployment as single index.html drop-in with no asset coordination | ✓ Good |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each milestone** (via `/gsd:complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-04-28 — Phase 07 complete (meta-hygiene: canonical, robots, Twitter Cards, robots.txt, sitemap.xml)*
