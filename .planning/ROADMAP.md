# Roadmap: Leadzly Website Overhaul

## Milestones

- ✅ **v1.0 Full Website Overhaul** — Phases 1–6 (shipped 2026-04-28)
- **v1.1 SEO & Performance** — Phases 7–9 (active)

## Phases

<details>
<summary>✅ v1.0 Full Website Overhaul (Phases 1–6) — SHIPPED 2026-04-28</summary>

- [x] Phase 1: Foundation & Hygiene (2/2 plans) — completed 2026-04-24
- [x] Phase 2: Page Restructure & Scaffolding (2/2 plans) — completed 2026-04-27
- [x] Phase 3: Trust & Social Proof (3/3 plans) — completed 2026-04-27
- [x] Phase 4: Conversion Sections (4/4 plans) — completed 2026-04-28
- [x] Phase 5: Booking Experience & CTAs (4/4 plans) — completed 2026-04-28
- [x] Phase 6: Design Polish & Mobile (2/2 plans) — completed 2026-04-28

Full details: [.planning/milestones/v1.0-ROADMAP.md](./milestones/v1.0-ROADMAP.md)

</details>

### v1.1 SEO & Performance

- [x] **Phase 7: Meta Hygiene** — Canonical, robots, Twitter Cards, robots.txt, sitemap.xml added to head and root (completed 2026-04-28)
- [ ] **Phase 8: Structured Data** — Organization/LocalBusiness, FAQPage, and Service JSON-LD blocks live in head
- [ ] **Phase 9: Performance & Core Web Vitals** — Font weights trimmed, SVGs lazy-loaded, CLS eliminated, CSS minified

---

## Phase Details

### Phase 7: Meta Hygiene
**Goal**: Search engines and social platforms can correctly index, follow, and preview leadzly.co
**Depends on**: Nothing (first phase of v1.1)
**Requirements**: META-01, META-02, META-03, META-04, META-05
**Success Criteria** (what must be TRUE):
  1. Google can confirm canonical URL via `<link rel="canonical">` — no duplicate-URL confusion
  2. Googlebot receives explicit `index, follow` directive in page head
  3. Sharing leadzly.co on Twitter/X renders a summary card with title, description, and image
  4. `robots.txt` is accessible at `https://leadzly.co/robots.txt` and references the sitemap URL
  5. `sitemap.xml` is accessible at `https://leadzly.co/sitemap.xml` with the homepage URL and a valid `<lastmod>` date
**Plans**: 2 plans
Plans:
- [x] 07-01-PLAN.md — Add canonical, robots meta, and Twitter Card tags to index.html head (META-01, META-02, META-03)
- [x] 07-02-PLAN.md — Create robots.txt and sitemap.xml at project root (META-04, META-05)
**UI hint**: no

### Phase 8: Structured Data
**Goal**: leadzly.co emits machine-readable schema that qualifies it for Google rich results (FAQ snippets, business knowledge panel)
**Depends on**: Phase 7
**Requirements**: SCHEMA-01, SCHEMA-02, SCHEMA-03
**Success Criteria** (what must be TRUE):
  1. Google's Rich Results Test confirms valid Organization + LocalBusiness markup (name, URL, logo, contactPoint, areaServed UK/NI)
  2. Google's Rich Results Test confirms valid FAQPage markup covering all 7 FAQ questions
  3. Google's Rich Results Test confirms valid Service markup for telesales, meeting booking, and BDM provision
  4. All three JSON-LD blocks are in `<head>` and produce zero validation errors in schema.org validator
**Plans**: 2 plans
Plans:
- [x] 08-01-PLAN.md — Insert Organization + LocalBusiness JSON-LD block into index.html head (SCHEMA-01)
- [ ] 08-02-PLAN.md — Insert FAQPage (7 Q&A) and Service (4 services) JSON-LD blocks into index.html head (SCHEMA-02, SCHEMA-03)
**UI hint**: no

### Phase 9: Performance & Core Web Vitals
**Goal**: leadzly.co scores 90+ on Lighthouse Performance with zero measurable Cumulative Layout Shift from controllable sources
**Depends on**: Phase 7
**Requirements**: PERF-01, PERF-02, PERF-03, PERF-04, PERF-05
**Success Criteria** (what must be TRUE):
  1. Google Fonts request loads only weights 400, 600, and 700 — no other weight variants fetched on network inspection
  2. Pipeline board and outreach sequence SVG mockups carry `loading="lazy"` and do not appear in the browser's initial render waterfall
  3. Calendly embed container has explicit `min-height` set — no layout shift is visible when the widget loads
  4. Floating CTA appears without any measurable page reflow — CLS score contribution from this element is 0
  5. `page.css` is served as a minified file with a version-bump query string or filename suffix
**Plans**: TBD
**UI hint**: no

---

## Progress

| Phase | Milestone | Plans Complete | Status | Completed |
|-------|-----------|----------------|--------|-----------|
| 1. Foundation & Hygiene | v1.0 | 2/2 | Complete | 2026-04-24 |
| 2. Page Restructure & Scaffolding | v1.0 | 2/2 | Complete | 2026-04-27 |
| 3. Trust & Social Proof | v1.0 | 3/3 | Complete | 2026-04-27 |
| 4. Conversion Sections | v1.0 | 4/4 | Complete | 2026-04-28 |
| 5. Booking Experience & CTAs | v1.0 | 4/4 | Complete | 2026-04-28 |
| 6. Design Polish & Mobile | v1.0 | 2/2 | Complete | 2026-04-28 |
| 7. Meta Hygiene | v1.1 | 2/2 | Complete   | 2026-04-28 |
| 8. Structured Data | v1.1 | 1/2 | In Progress|  |
| 9. Performance & Core Web Vitals | v1.1 | 0/? | Not started | — |

---
*v1.0 shipped: 2026-04-28 | v1.1 active — Phase 8 planned, ready to execute*
