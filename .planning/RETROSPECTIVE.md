# Retrospective

---

## Milestone: v1.0 — Full Website Overhaul

**Shipped:** 2026-04-28
**Phases:** 6 | **Plans:** 15 | **Commits:** ~112

### What Was Built

- OG tags, favicon, and GDPR cookie consent banner with real GA4 gating
- 14-section page restructure — pain points, multi-channel, industries, scaffold shells
- Section background alternation + full card CSS system
- Trust bar with partner logos, stat badge, 30-day guarantee
- Proven Across Industries — 4 outcome cards (testimonials intentionally deleted)
- About section repositioned higher for credibility-first flow
- Typography polish — hero h1 clamp, green eyebrow labels on all sections
- Comparison table (8 rows, Leadzly column highlighted), FAQ accordion (7 questions), ROI calculator (client-side JS)
- Inline Calendly booking embed, floating sticky CTA with pulse animation
- Hero stat counter animation (count-up on scroll)
- Card hover lift + scroll fade-up animations across all 13 below-fold sections
- Dashboard-style SVG illustrations — CRM pipeline board (How It Works) + outreach sequence timeline (Multi-Channel)

### What Worked

- **IIFE pattern** for all JS kept the site as a single drop-in HTML file — zero coordination overhead for deployment
- **Human checkpoint gates** in plans caught visual regressions before they reached main (comparison table colour revert, floating CTA mobile overlap)
- **Functional-only SVG illustrations** added genuine visual weight without decorative noise
- **Design tokens throughout** — brand colours and spacing as CSS variables made consistent edits fast

### What Was Inefficient

- Phase 1 was executed first but STATE.md phase log never reflected it as complete — caused confusion at milestone close
- Some phase SUMMARY.md files were created with `one_liner:` stub rather than a real one-liner — degraded MILESTONES.md auto-extraction
- Quick task executor timed out mid-run; the work had completed but the timeout caused a false failure signal

### Patterns Established

- `display: contents` wrapper pattern for conditional desktop split layouts (channels section)
- Inline `<style>` blocks in `<head>` for component-scoped CSS where page.css separation adds overhead
- Double-rAF pattern for CSS transitions after `hidden` attribute removal
- `observer.disconnect()` after first IntersectionObserver trigger — prevents replay on re-scroll
- All CTAs unified to a small set of variants — single source of truth prevents copy drift

### Key Lessons

- Delete empty scaffold sections rather than leaving placeholders — fake social proof (testimonials) degrades credibility more than absence
- Verify REQUIREMENTS.md traceability table after each phase, not just at milestone close
- Confirm phase summaries include non-empty `one_liner` field — the CLI extraction depends on it

### Cost Observations

- Model: Sonnet 4.6 throughout
- Sessions: ~6 across 5 days (2026-04-24 → 2026-04-28)
- Notable: Quick tasks used worktree isolation effectively; executor timeouts were benign (work completed before timeout)

---

## Milestone: v1.1 — SEO & Performance

**Shipped:** 2026-04-29
**Phases:** 3 | **Plans:** 6 | **Commits:** ~32 | **Duration:** 2 days

### What Was Built

- Canonical URL, meta robots (index/follow), and 4 Twitter Card tags added to `<head>`
- `robots.txt` (allow-all + Sitemap pointer) and `sitemap.xml` at project root
- Organization + LocalBusiness JSON-LD with areaServed UK/NI and Calendly contactPoint
- FAQPage JSON-LD mapping all 7 FAQ questions; Service JSON-LD covering 4 service types
- Google Fonts trimmed from 9 to 7 weight variants; 5 CSS `font-weight: 500` rules bumped to 600
- Both inline SVG mockups (~320 lines) externalised as standalone files, lazy-loaded via `<img>`
- Calendly container pre-reserved at 700px / 650px to eliminate CLS
- `page.css` minified from 1,693 lines to a single line via CleanCSS DEFAULT; version-busted to `?v=14`

### What Worked

- **Human-checkpoint pattern** for CSS minification — pasting into CleanCSS.com was the right call, automated minification would have risked stripping CSS custom properties
- **Interfaces blocks in plans** — pre-extracted exact line numbers and current CSS values meant executors made zero exploratory reads; tasks executed in one pass
- **Parallel Wave 1** completed in a single agent run with no deviations from the plan
- **PERF-04 satisfied by absence** — confirming the floating CTA was already gone via grep was faster and safer than any code change

### What Was Inefficient

- Phase 09 plan 02 required a human checkpoint for CSS minification — a CleanCSS CLI integration would make this fully automatable in future
- No milestone audit run before completion; skipped because all requirements were manually confirmed, but a formal audit would have verified cross-phase integration (structured data referencing the correct canonical URL, etc.)

### Patterns Established

- CleanCSS DEFAULT (not "advanced") is the correct minification level for CSS with custom properties
- External SVG files + `loading="lazy"` with explicit `width`/`height` attributes is the correct pattern for decorative illustrations — avoids CLS and reduces HTML parse cost
- `min-height` (not `height`) on Calendly container allows widget to expand past the reserved height

### Key Lessons

- Run the milestone audit before completing — cross-phase integration checks (schema URLs matching canonical, etc.) are worth the 5 minutes
- `wc -l` returning 0 for a minified CSS file is expected and correct — single-line files have no newlines
- Font weight 500 must be audited when trimming a font URL — CSS rules referencing that weight need updating simultaneously or the browser will silently fall back

### Cost Observations

- Model: Sonnet 4.6 throughout
- Sessions: 2 sessions across 2 days (2026-04-28 → 2026-04-29)
- Notable: Short milestone (3 phases, 6 plans) completed in 2 days with minimal deviations

---

## Cross-Milestone Trends

| Milestone | Phases | Plans | Commits | Duration | Key Pattern |
|-----------|--------|-------|---------|----------|-------------|
| v1.0 | 6 | 15 | ~112 | 5 days | IIFE-only JS, inline SVG illustrations |
| v1.1 | 3 | 6 | ~32 | 2 days | Human checkpoint for CSS minification, CleanCSS DEFAULT level |
