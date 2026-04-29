# Leadzly Website Overhaul

## Current Milestone: v1.2 E-E-A-T & AI Search Signals

**Goal:** Strengthen leadzly.co's authority and AI citability by adding real contact details, founder identity, additional schema, and AEO-optimised copy — all within the existing static page.

**Target features:**
- NAP on page: full postal address (1 Hollycroft Avenue, Belfast, BT5 5JE) and WhatsApp phone in footer + LocalBusiness schema
- Founder signals: Allan Chan name and LinkedIn link in About section + Person JSON-LD
- Schema additions: HowTo JSON-LD for How It Works section; LocalBusiness updated with full address and telephone
- AEO/LLMO copy: FAQ answers restructured for direct AI extraction; hero and services copy with specific citable statistics

## Current State

**v1.1 shipped: 2026-04-29** — Both milestones complete. leadzly.co is a fully rebuilt, conversion-optimised, SEO-ready B2B landing page live on Vercel via RoseyCoUk/leadzly-new.

---

## What This Is

A fully redesigned and conversion-optimised landing page for Leadzly (leadzly.co) — a B2B outbound sales agency based in Northern Ireland. The static HTML/CSS/JS page was rebuilt from a bare-bones site into a full-funnel conversion page with trust signals, a 14-section flow, inline booking, interactive tools (ROI calculator, FAQ, comparison table), scroll animations, and dashboard-style SVG illustrations. It is now crawlable, indexable, schema-eligible, and performance-optimised.

## Core Value

A prospect visiting leadzly.co should leave with enough trust and urgency to book a strategy call — the page must convert sceptical UK/NI B2B decision-makers.

## Requirements

### Validated (v1.0)

- ✓ Sticky nav with blur/shadow on scroll (NAV-01) — v1.0
- ✓ Hamburger menu on mobile (NAV-02) — v1.0
- ✓ All external links `rel="noopener noreferrer"` (NAV-03) — v1.0
- ✓ OG tags (title, description, image, URL) and favicon (QWIN-01, QWIN-02) — v1.0
- ✓ GDPR cookie consent banner with GA4 gating (QWIN-04) — v1.0
- ✓ 14-section page structure in correct order (PAGE-01) — v1.0
- ✓ Pain points section — 6 empathy cards (PAGE-02) — v1.0
- ✓ Multi-channel breakdown — 4 channel cards (PAGE-03) — v1.0
- ✓ Industries served — 12-chip grid with icons (PAGE-04) — v1.0
- ✓ Section background alternation (DSGN-02) — v1.0
- ✓ Trust bar below hero — logos, stat badge, guarantee (TRST-05) — v1.0
- ✓ Proven Across Industries — 4 outcome cards (TRST-03, TRST-04) — v1.0
- ✓ About section, NI-first framing (TRST-06) — v1.0
- ✓ Hero h1 font-weight 800, clamp ≥3.5rem; green eyebrow labels (DSGN-01) — v1.0
- ✓ Comparison table — Leadzly vs In-House vs Other Agencies (SECT-01) — v1.0
- ✓ FAQ accordion — 7 questions (SECT-02) — v1.0
- ✓ ROI calculator — 3 inputs, client-side JS (SECT-03) — v1.0
- ✓ Inline Calendly embed — two-column layout (BOOK-01) — v1.0
- ✓ Floating sticky CTA — pulsing dot, 3s delay (BOOK-02) — v1.0
- ✓ Varied CTA copy across sections (BOOK-03) — v1.0
- ✓ Counter animation in hero (QWIN-03) — v1.0
- ✓ Card hover lift + scroll fade animations (DSGN-03) — v1.0
- ✓ Full mobile responsiveness (DSGN-04) — v1.0
- ✓ Dashboard-style SVG illustrations (quick task) — v1.0

### Validated (v1.1)

- ✓ Canonical URL tag (META-01) — v1.1
- ✓ Meta robots tag (META-02) — v1.1
- ✓ Twitter Card meta tags (META-03) — v1.1
- ✓ robots.txt (META-04) — v1.1
- ✓ sitemap.xml (META-05) — v1.1
- ✓ Organization + LocalBusiness JSON-LD (SCHEMA-01) — v1.1
- ✓ FAQPage JSON-LD — 7 Q&A pairs (SCHEMA-02) — v1.1
- ✓ Service JSON-LD — 4 services (SCHEMA-03) — v1.1
- ✓ Font weight trim — 400/600/700 only (PERF-01) — v1.1
- ✓ SVG mockups lazy-loaded as external files (PERF-02) — v1.1
- ✓ Calendly embed CLS eliminated via min-height (PERF-03) — v1.1
- ✓ Floating CTA zero CLS contribution (PERF-04) — v1.1
- ✓ page.css minified + cache-bust v=14 (PERF-05) — v1.1

### Dropped (intentional)

- ✗ TRST-01/02 — Testimonials — *no real content available; fake social proof undermines credibility*

### Active (v1.2 candidates)

- [ ] Testimonials section — when real client testimonials are sourced (TRST-01/02 revival)
- [ ] Replace stock hero image with NI/UK-specific B2B photography (V2-05)
- [ ] Team photos in About section (V2-04, blocked on asset sourcing)
- [ ] Live Clutch review widget if API becomes available (V2-01)
- [ ] Active section highlighting in nav on scroll (V2-02)
- [ ] Phone number in nav (V2-03)

### Out of Scope

- CMS or backend — static site only
- React, Vue, or any JS framework — vanilla JS only, no build step
- External third-party libraries beyond Calendly embed script
- Separate pages or subdomains — single-page only
- SEO content writing / blog — copy improvements within existing sections only
- Google Search Console submission — operational task
- Backlink building / off-page SEO — no code changes

## Context

- **Live URL:** leadzly.co (deployed via Vercel — RoseyCoUk/leadzly-new)
- **Stack:** `index.html` + `base.css`, `page.css` (minified) + vanilla JS (inline IIFEs)
- **Size:** index.html ~1,500 LOC; page.css single-line minified (~35KB); base.css ~200 LOC
- **Brand:** Green `#16a34a`, Dark navy `#0f172a`, Light green `#22c55e`, Green bg `#f0fdf4`; Fonts: Inter + Plus Jakarta Sans (Google Fonts, weights 400/600/700/800)
- **Calendly URL:** https://calendly.com/allan-chan-leadzly
- **Logos:** `Leadzly-logo_with-text.svg` and `Leadzly-logo_without-text.svg` in project root
- **SVG mockups:** `outreach-sequence.svg`, `pipeline-board.svg` (externalised in v1.1)
- **SEO files:** `robots.txt`, `sitemap.xml` at project root (added in v1.1)
- **Competitor benchmarks:** Belkins, SalesRoads, Martal Group
- **Key differentiator:** NI-based team, no offshore — genuine advantage for UK clients burned by overseas call centres
- **GA4:** Installed via Partytown, gated behind GDPR consent banner

## Constraints

- **Tech stack:** HTML/CSS/vanilla JS only — no build system, no npm, no frameworks
- **Calendly:** Inline embed uses Calendly widget script (loadable from CDN), no API key needed
- **Fonts:** Inter + Plus Jakarta Sans via Google Fonts — no new font imports
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
| Light-only palette: white / #f8fafb / #f0fdf4 | Three tones only — clean, professional | ✓ Good |
| Green solid nav (#16a34a) | Immediately anchors brand colour | ✓ Good |
| Multi-channel section: light not dark | Dark mid-page sections broke clean light palette | ✓ Good |
| Comparison table: light green not dark navy | Consistent with page light theme | ✓ Good |
| Hero: split layout + meeting card SVG | No real photos; meeting card communicates value visually | ✓ Good |
| About section between Services and Multi-Channel | Credibility narrative precedes evidence sections | ✓ Good |
| Calendly height 700px desktop / 650px mobile | Eliminates internal scrollbar in widget | ✓ Good |
| Counter animation — play once per load | observer.disconnect() after first trigger | ✓ Good |
| SVG mockups externalised (v1.1) | Lazy-loading reduces initial HTML parse by ~320 lines | ✓ Good |
| CleanCSS DEFAULT level for minification | Preserves var(--) custom properties; "advanced" strips them | ✓ Good |
| Inline IIFEs, no separate JS file | Single index.html drop-in, no asset coordination | ✓ Good |

---
*Last updated: 2026-04-29 — v1.2 milestone started (E-E-A-T & AI Search Signals)*
