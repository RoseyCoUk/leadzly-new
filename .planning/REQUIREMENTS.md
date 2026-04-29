# Requirements — Leadzly Website Overhaul

## Milestone v1.1: SEO & Performance

**Goal:** Make leadzly.co crawlable, indexable, and fast enough to rank — structured data, meta hygiene, and Core Web Vitals fixes.

---

### Meta Hygiene

- [x] **META-01**: Site includes `<link rel="canonical" href="https://leadzly.co/">` in `<head>`
- [x] **META-02**: Site includes `<meta name="robots" content="index, follow">` in `<head>`
- [x] **META-03**: Site includes all 4 Twitter Card meta tags (`twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`)
- [x] **META-04**: `robots.txt` exists at site root, allowing all crawlers and referencing sitemap URL
- [x] **META-05**: `sitemap.xml` exists at site root with homepage URL and `<lastmod>` date

### Structured Data

- [x] **SCHEMA-01**: Organization + LocalBusiness JSON-LD block in `<head>` (name, URL, logo, contactPoint, areaServed UK/NI)
- [ ] **SCHEMA-02**: FAQPage JSON-LD block mapping all 7 existing FAQ questions and answers
- [ ] **SCHEMA-03**: Service JSON-LD block covering telesales, meeting booking, and BDM provision offerings

### Performance (Core Web Vitals)

- [ ] **PERF-01**: Google Fonts request trimmed to weights 400, 600, 700 only (removes 5 unused weight variants)
- [ ] **PERF-02**: Below-fold decorative SVG mockups (pipeline board, outreach sequence) lazy-loaded — no longer blocking initial HTML parse
- [ ] **PERF-03**: Calendly embed container given explicit `min-height` so no layout shift on load
- [ ] **PERF-04**: Floating CTA inject causes zero CLS (container pre-reserved or transition adjusted)
- [ ] **PERF-05**: `page.css` minified — served as minified file, version-bumped

---

## Future Requirements

- Testimonials section — when real client testimonials are sourced (TRST-01/02 revival)
- Active section highlighting in nav on scroll (V2-02)
- Phone number in nav (V2-03)
- Team photos in About section (V2-04, blocked on asset sourcing)
- Replace stock hero image with NI/UK-specific B2B photography (V2-05)

## Out of Scope

- Google Search Console submission — operational task, not a code change
- Backlink building / off-page SEO — no code changes involved
- Blog or additional pages — single-page improvements only (existing constraint)
- Analytics / conversion tracking changes — GA4 already installed via Partytown

---

## Traceability

| REQ-ID | Phase | Plan |
|--------|-------|------|
| META-01 | Phase 7 | — |
| META-02 | Phase 7 | — |
| META-03 | Phase 7 | — |
| META-04 | Phase 7 | — |
| META-05 | Phase 7 | — |
| SCHEMA-01 | Phase 8 | — |
| SCHEMA-02 | Phase 8 | — |
| SCHEMA-03 | Phase 8 | — |
| PERF-01 | Phase 9 | — |
| PERF-02 | Phase 9 | — |
| PERF-03 | Phase 9 | — |
| PERF-04 | Phase 9 | — |
| PERF-05 | Phase 9 | — |
