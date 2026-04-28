---
name: Add graphics to site
description: Add dashboard-style inline SVG mockup illustrations to selected sections
type: project
---

# Quick Task 260428-hbj: Add graphics to the site to bring it to life - Context

**Gathered:** 2026-04-28
**Status:** Ready for planning

<domain>
## Task Boundary

Add dashboard/UI mockup-style inline SVG illustrations to selected sections of index.html. These should visually demonstrate what each section is describing — similar in concept to the hero section which already has a dashboard-style mockup visual.

</domain>

<decisions>
## Implementation Decisions

### Graphic style
- Inline SVG only — no external images, no CDN assets
- Dashboard/UI mockup style (fake CRM screens, metrics cards, workflow visuals)
- Reference: hero section already uses this style

### Which sections
- Only sections where a visual genuinely reinforces the content — not every section
- Hero already has a graphic — do not touch
- Focus on sections where "showing" the concept adds conversion value
- Primary candidates: How It Works, Multi-Channel Outreach, Proven/Results

### Decoration depth
- Functional only — every graphic must reinforce section meaning
- No purely decorative blobs, backgrounds, or abstract shapes
- Visuals should look like product/process mockups, not generic illustrations

### Tech constraints
- HTML/CSS/vanilla JS only — no build system, no npm
- No external image files — all visuals are inline SVG in index.html
- No new font imports — use existing Inter via Google Fonts
- No new external dependencies

### Claude's Discretion
- Exact SVG design for each illustration (color palette should match site brand)
- Whether to add 2-column layout (text left, mockup right) or stack below content
- Final section count if some sections genuinely don't benefit from a visual

</decisions>

<specifics>
## Specific Ideas

- How It Works: pipeline/workflow dashboard mockup showing the 4 steps as a visual CRM flow
- Multi-Channel Outreach: outreach sequence UI mockup showing email/LinkedIn/call touchpoints
- Proven/Results: metrics dashboard cards showing results data (meetings booked, pipeline value, etc.)
- Style reference: hero section's existing visual treatment

</specifics>

<canonical_refs>
## Canonical References

No external specs — requirements fully captured in decisions above.

</canonical_refs>
