# Milestones

## v1.1 SEO & Performance (Shipped: 2026-04-29)

**Phases completed:** 3 phases, 6 plans, 13 tasks

**Key accomplishments:**

- Canonical URL, robots index/follow directive, and four Twitter Card meta tags added to index.html head — Google confirms preferred URL, all crawlers receive explicit index directive, X/Twitter renders branded summary card
- Static robots.txt (allow-all + Sitemap pointer) and sitemap.xml (sitemaps.org 0.9, single homepage URL with lastmod) created at project root
- Organization + LocalBusiness JSON-LD block with @type ["LocalBusiness","ProfessionalService"], areaServed UK/NI, and Calendly contactPoint inserted into index.html head
- FAQPage JSON-LD (7 Q&A pairs) and Service JSON-LD (@graph of 4 services) inserted into index.html head, completing all three SCHEMA-01/02/03 requirements for Phase 8
- Google Fonts trimmed to 7 weight variants, both inline SVG mockups externalised as lazy-loaded img files (~320 lines removed from HTML), and Calendly container pre-reserves 700px/650px to eliminate layout shift
- page.css minified from 1,693 lines to a single line via CleanCSS DEFAULT level, with cache-bust version bump to ?v=14 and PERF-04 (floating CTA CLS absence) confirmed by grep

---

## v1.0 Full Website Overhaul (Shipped: 2026-04-28)

**Phases completed:** 6 phases, 15 plans, 26 tasks

**Key accomplishments:**

- OG image, url, type, and alt meta tags + SVG/PNG favicon links added to index.html head; NAV requirements confirmed live
- Dark-navy consent banner with Accept-only controls, localStorage persistence, and real GA4 gating blocking analytics until user accepts
- 14-section index.html DOM order established with pain-points (6 cards), channels (4 cards), industries (12 chips), and 4 scaffold shells for testimonials, comparison, FAQ, and booking
- DSGN-02 background alternation applied to all 14 sections, with full card CSS for pain points (light grey), multi-channel (dark navy with white text), and industries (green-tinted chips)
- Greyscale partner logo strip with stat and guarantee badges inserted as a slim trust band directly below the hero section
- Four-card industry evidence block with green tag chips, bold outcomes, and muted detail lines, replacing the case-studies scaffold — testimonials scaffold deleted entirely
- About section cut from after Pricing and inserted between Services and Multi-Channel — pure HTML repositioning with id=about intact for nav/footer anchor links
- Hero h1 clamp minimum raised from 2.4rem to 3.5rem and all section eyebrow labels hardened to font-weight 700 in page.css
- One-liner:
- 7-question FAQ accordion with CSS max-height animation, chevron rotation, and vanilla JS IIFE one-at-a-time toggle — SECT-02 satisfied
- One-liner:
- One-liner:
- One-liner:
- Hero card numbers (12, 94%, 8) count up from 0 on first scroll-into-view; floating sticky CTA fixed and now live on main branch
- CSS-only card hover lift (translateY(-4px) + green border + elevated shadow) added to all 8 card classes, and .fade-in animation upgraded to include translateY(20px) slide-up alongside opacity fade
- fade-in class added to all 13 below-fold sections in index.html — sections now slide up from 20px into position as the user scrolls, with human sign-off on Phase 6 full visual quality

---
