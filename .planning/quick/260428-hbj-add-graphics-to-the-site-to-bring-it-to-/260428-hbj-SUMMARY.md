---
phase: quick
plan: 260428-hbj
subsystem: landing-page-visuals
tags: [svg, illustrations, dashboard-mockups, desktop-only, inline-svg]
dependency_graph:
  requires: []
  provides: [how__visual, channels__visual, proven__visual]
  affects: [index.html, page.css]
tech_stack:
  added: []
  patterns: [inline-SVG-mockups, display-contents-wrapper, mobile-first-hidden-visual]
key_files:
  created: []
  modified:
    - index.html
    - page.css
decisions:
  - channels__split uses display:contents on mobile so it is transparent to existing layout
  - channels__grid overridden back to repeat(2,1fr) inside split at >=1024px to prevent 4-col overflow in half-width container
  - SVG viewBox coordinates use fixed units; all text uses Inter,sans-serif font-family fallback
  - Metrics in proven dashboard use live-looking values consistent with page copy (47 meetings, £94k pipeline, 94% show rate)
metrics:
  duration: ~8 minutes
  completed: 2026-04-28
  tasks_completed: 3
  files_modified: 2
---

# Quick Task 260428-hbj: Add Dashboard Graphics to Landing Page

**One-liner:** Three inline SVG dashboard mockups added to How It Works (CRM pipeline board), Multi-Channel Outreach (outreach sequence timeline), and Proven/Results (metrics dashboard) — all hidden on mobile, visible on desktop.

---

## What Was Built

### Task 1 — How It Works: CRM Pipeline Board SVG (ffc028f)

A 4-column CRM pipeline board mockup inserted as `.how__visual` after the `.how__timeline` div.

**SVG design (640x210 viewBox):**
- White card with green 3px top bar and `#e2e8f0` border, rx12
- Four column headers (ICP Audit, Scripting, Live Outreach, Meetings Booked) as green-tinted pills
- Vertical dividers between columns
- Lead row rectangles (`#f8fafc` fill) in the first three columns with placeholder company names
- Meetings Booked column uses `#dcfce7` green tint rows with a `#16a34a` check-circle (green circle, white polyline checkmark)
- Footer bar showing "3 leads in pipeline" and a live dot

**CSS:** `display: none` by default; `display: flex; justify-content: center` at `min-width: 900px`. Drop-shadow filter, `max-width: 640px`, `border-radius: 12px`.

---

### Task 2 — Multi-Channel Outreach: Sequence Timeline UI SVG (44e4593)

A 5-touchpoint outreach sequence card inserted as `.channels__visual` to the right of the channel cards. The `.channels__grid` was wrapped in a `.channels__split` div to create the two-column desktop layout.

**SVG design (340x310 viewBox):**
- White card with green top bar, "Outreach Sequence" header + green "Active" dot
- 5 timeline rows (Day 1 Call / Day 3 Email / Day 5 LinkedIn / Day 8 Email / Day 12 Call)
  - Each row: day badge (green tint), channel icon circle, channel label, status pill
  - Rows 1-3: "Sent" pill (`#dcfce7` / `#16a34a`)
  - Rows 4-5: "Scheduled" pill (`#f1f5f9` / `#64748b`)
  - Dashed connector lines between rows
- Stats section: Open Rate 74%, Reply Rate 36%, Booked 19% — horizontal progress bars in green

**CSS wrapper approach:**
- `.channels__split { display: contents }` below 1024px — fully transparent to existing channel card layout
- At `min-width: 1024px`: switches to `display: grid; grid-template-columns: 1fr 1fr`
- Scoped override `.channels__split .channels__grid { grid-template-columns: repeat(2, 1fr) }` at 1024px — prevents the 4-column grid rule from overflowing inside the 1fr left column

---

### Task 3 — Proven/Results: Metrics Dashboard SVG (287c859)

A 4-metric campaign dashboard inserted as `.proven__visual` between the `.section-header` and `.proven__grid`.

**SVG design (640x190 viewBox):**
- White card with green top bar, "Campaign Dashboard" header + "This Month" badge
- Four metric cards (138-146px wide, 88px tall, rx8, `#f8fafc` fill):
  - 47 Meetings Booked (dark number, green up-arrow, +12%)
  - £94k Pipeline Value (dark number, green up-arrow, +8%)
  - 12 Industries (dark number, no arrow — stable stat)
  - 94% Show Rate (green number, green up-arrow, +3%)
- 5-bar mini bar chart (Mon–Fri) at bottom, bars in `#16a34a` at varying heights, 9px day labels

**CSS:** `display: none` by default; `display: flex; justify-content: center` at `min-width: 768px`. Drop-shadow filter, `max-width: 640px`.

---

## Deviations from Plan

### Auto-fixed Issues

**1. [Rule 1 - Bug] channels__grid 4-column override conflicts with channels__split**
- **Found during:** Task 2
- **Issue:** Page.css already had `@media (min-width: 1024px) { .channels__grid { grid-template-columns: repeat(4, 1fr); } }`. Inside the 1fr column of `.channels__split`, this would force all 4 channel cards into a single narrow column overflowing the container.
- **Fix:** Added scoped override `.channels__split .channels__grid { grid-template-columns: repeat(2, 1fr); }` inside the 1024px media query, restoring a clean 2x2 grid within the split layout.
- **Files modified:** page.css
- **Commit:** 44e4593

---

## Known Stubs

None — all SVG data values (47 meetings, £94k, 94% show rate) match the marketing copy already present on the page. No placeholder text or TODO markers in SVG content.

---

## Verification Checklist

- [x] `.how__visual` div exists in index.html with inline SVG after `.how__timeline`
- [x] `.channels__visual` div exists inside `.channels__split` wrapper
- [x] `.proven__visual` div exists before `.proven__grid`
- [x] All three SVGs have `aria-hidden="true"` and `role="presentation"`
- [x] All three use `display: none` by default
- [x] `.how__visual` visible at `min-width: 900px`
- [x] `.channels__visual` visible at `min-width: 1024px`
- [x] `.proven__visual` visible at `min-width: 768px`
- [x] Hero section (`.hero__visual`) untouched — confirmed line 168
- [x] Brand colours used: `#16a34a`, `#0f172a`, `#f0fdf4`, `#dcfce7`
- [x] No new external images, fonts, or scripts referenced
- [x] No new files created (SVGs inline in index.html)

---

## Revisions (post human-verify)

- Removed `.proven__visual` metrics dashboard from Proven Across Industries section (user feedback — not needed there)
- Extended How It Works CRM board viewBox from 640×380 → 640×500, added 5th row of pipeline cards across all 4 columns, added stats footer bar
- Removed `max-width: 480px` cap from `.channels__visual svg` — now fills full column width at desktop
- Increased How It Works max-width from 860px → 960px

## Final Commits

- `ffc028f` feat: add CRM pipeline board SVG to How It Works
- `44e4593` feat: add outreach sequence UI mockup to Multi-Channel section
- `287c859` feat: add metrics dashboard SVG to Proven/Results
- `5d5296e` fix: remove proven dashboard, enlarge CRM pipeline and outreach sequence visuals
- `9126d98` fix: extend HIT pipeline board to 640×500, remove channels max-width cap

## Self-Check: PASSED
