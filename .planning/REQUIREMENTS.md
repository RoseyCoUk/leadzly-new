# Requirements: Leadzly Website Overhaul

**Defined:** 2026-04-24
**Core Value:** A prospect visiting leadzly.co should leave with enough trust and urgency to book a strategy call — the page must convert sceptical UK/NI B2B decision-makers.

## v1 Requirements

### Trust & Social Proof

- [ ] **TRST-01**: Page displays 8–12 testimonial cards each with name, job title, company type, location, and star rating
- [ ] **TRST-02**: Testimonials are laid out in a 3-column grid with a carousel/scroll on mobile
- [ ] **TRST-03**: Page displays 4–5 case study cards each with industry tag, timeframe badge, 2–3 key metrics, problem→action→result story, and a client quote
- [ ] **TRST-04**: Case study cards have a dark header showing industry and metrics
- [ ] **TRST-05**: A trust bar appears directly below the hero with: Clutch rating, "Rated #1 Sales Agency NI" badge, greyscale client logo strip, and "30-Day Guarantee" badge
- [ ] **TRST-06**: About section is moved higher up the page and emphasises the NI-based, no-offshore team angle

### Page Structure & Flow

- [x] **PAGE-01**: Page sections are ordered: hero → trust bar → pain points → services → multi-channel → process → industries → case studies → testimonials → comparison table → pricing → FAQ → booking → footer CTA → footer
- [x] **PAGE-02**: Pain points section exists with headline "Does this sound familiar?" and 6 empathetic problem cards (empty pipeline, closers stuck prospecting, SDR hire cost/risk, unqualified leads, burned by agency, no outbound visibility)
- [x] **PAGE-03**: Multi-channel breakdown section exists on dark navy background with headline and 4 channel cards (Telesales, Cold Email, LinkedIn, Follow-Up)
- [x] **PAGE-04**: Industries served section exists with a 12-chip grid (icon + label) covering SaaS, Recruitment, E-commerce, Professional Services, Financial Services, Construction, Healthcare, Logistics, Education, Marketing Agencies, Managed IT, Energy

### Booking Experience

- [ ] **BOOK-01**: An inline Calendly embed section exists with a two-column layout (left: sell the call with bullet benefits; right: live calendar widget)
- [ ] **BOOK-02**: A floating sticky CTA button exists in the bottom-right corner with a pulsing green dot, animates in after 3 seconds, and hides on mobile if overlapping content
- [ ] **BOOK-03**: Every major section includes a CTA with varied copy ("Book a Free Strategy Call", "Get Your Meetings Forecast", "See If We're a Fit", "Start Filling Your Calendar")

### Design Polish

- [ ] **DSGN-01**: Hero h1 uses font-weight 900 and clamps to at least 3.5rem; every section has a consistent eyebrow label (small caps, green) above the heading
- [x] **DSGN-02**: Page sections alternate between white, light grey (#fafafa), dark navy (#0f172a), and green-tinted (#f0fdf4) backgrounds to create visual rhythm
- [ ] **DSGN-03**: All cards lift on hover (translateY(-4px) + box-shadow) and show a green border highlight; sections fade up on scroll via IntersectionObserver
- [ ] **DSGN-04**: All grids collapse to single column on mobile; process steps stack vertically without arrows; comparison table scrolls horizontally on small screens

### New Sections

- [ ] **SECT-01**: Comparison table section (dark navy background) with rows comparing Leadzly vs In-House vs Other Agencies across 8 criteria, Leadzly column highlighted in green
- [ ] **SECT-02**: FAQ accordion with 7 questions, one open at a time, max-width 720px centred
- [ ] **SECT-03**: ROI calculator with 3 inputs (average deal value, current meetings/month, target meetings/month) and outputs (projected additional monthly revenue, projected annual pipeline), all client-side JS

### Navigation & Global

- [ ] **NAV-01**: Nav becomes sticky with blur/shadow effect on scroll
- [ ] **NAV-02**: Hamburger menu exists on mobile
- [ ] **NAV-03**: All external links have `rel="noopener noreferrer"`

### Quick Wins

- [ ] **QWIN-01**: Page has `<meta>` description, Open Graph tags (title, description, image, URL)
- [ ] **QWIN-02**: Page has a custom favicon
- [ ] **QWIN-03**: Hero stat numbers count up via animation when scrolled into view
- [ ] **QWIN-04**: A GDPR-compliant cookie/privacy consent banner exists

## v2 Requirements

### Future Enhancements

- **V2-01**: Live Clutch review widget integration (requires Clutch API)
- **V2-02**: Active section highlighting in nav links on scroll
- **V2-03**: Phone number added to nav for trust
- **V2-04**: Team photos in About section (currently blocked on sourcing assets)
- **V2-05**: Replace stock hero image with NI/UK-specific B2B photography

## Out of Scope

| Feature | Reason |
|---------|--------|
| CMS or backend | Static site only — no server-side logic |
| React/Vue/JS framework | No build step — vanilla JS and HTML/CSS only |
| Third-party JS libraries (e.g. Swiper) | Keep dependencies minimal |
| Separate pages or subdomains | Single-page improvements only |
| SEO content writing / blog | Copy improvements within existing sections only |
| Google Analytics / Plausible | GA4 already installed via Partytown (recent commit) |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| NAV-01 | Phase 1 — Foundation & Hygiene | Pending |
| NAV-02 | Phase 1 — Foundation & Hygiene | Pending |
| NAV-03 | Phase 1 — Foundation & Hygiene | Pending |
| QWIN-01 | Phase 1 — Foundation & Hygiene | Pending |
| QWIN-02 | Phase 1 — Foundation & Hygiene | Pending |
| QWIN-04 | Phase 1 — Foundation & Hygiene | Pending |
| PAGE-01 | Phase 2 — Page Restructure & Scaffolding | Complete |
| PAGE-02 | Phase 2 — Page Restructure & Scaffolding | Complete |
| PAGE-03 | Phase 2 — Page Restructure & Scaffolding | Complete |
| PAGE-04 | Phase 2 — Page Restructure & Scaffolding | Complete |
| DSGN-02 | Phase 2 — Page Restructure & Scaffolding | Complete |
| TRST-01 | Phase 3 — Trust & Social Proof | Pending |
| TRST-02 | Phase 3 — Trust & Social Proof | Pending |
| TRST-03 | Phase 3 — Trust & Social Proof | Pending |
| TRST-04 | Phase 3 — Trust & Social Proof | Pending |
| TRST-05 | Phase 3 — Trust & Social Proof | Pending |
| TRST-06 | Phase 3 — Trust & Social Proof | Pending |
| SECT-01 | Phase 4 — Conversion Sections | Pending |
| SECT-02 | Phase 4 — Conversion Sections | Pending |
| SECT-03 | Phase 4 — Conversion Sections | Pending |
| DSGN-01 | Phase 4 — Conversion Sections | Pending |
| BOOK-01 | Phase 5 — Booking Experience & CTAs | Pending |
| BOOK-02 | Phase 5 — Booking Experience & CTAs | Pending |
| BOOK-03 | Phase 5 — Booking Experience & CTAs | Pending |
| QWIN-03 | Phase 5 — Booking Experience & CTAs | Pending |
| DSGN-03 | Phase 6 — Design Polish & Mobile | Pending |
| DSGN-04 | Phase 6 — Design Polish & Mobile | Pending |

**Coverage:**
- v1 requirements: 27 total
- Mapped to phases: 27
- Unmapped: 0

---
*Requirements defined: 2026-04-24*
*Last updated: 2026-04-24 after roadmap creation*
