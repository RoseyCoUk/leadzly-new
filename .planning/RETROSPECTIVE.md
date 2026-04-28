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

## Cross-Milestone Trends

| Milestone | Phases | Plans | Commits | Duration | Key Pattern |
|-----------|--------|-------|---------|----------|-------------|
| v1.0 | 6 | 15 | ~112 | 5 days | IIFE-only JS, inline SVG illustrations |
