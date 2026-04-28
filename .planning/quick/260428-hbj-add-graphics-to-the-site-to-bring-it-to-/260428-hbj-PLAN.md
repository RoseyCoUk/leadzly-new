---
phase: quick
plan: 260428-hbj
type: execute
wave: 1
depends_on: []
files_modified:
  - index.html
  - page.css
autonomous: true
requirements: []

must_haves:
  truths:
    - "How It Works section shows a pipeline/CRM flow mockup SVG beneath the 4 steps"
    - "Multi-Channel Outreach section shows an outreach sequence UI mockup SVG alongside the channel cards"
    - "Proven/Results section shows a metrics dashboard mockup SVG above or alongside the case study cards"
    - "All three SVG mockups use site brand colours (#16a34a, #0f172a, #f0fdf4) and Inter font"
    - "Mockups are hidden at mobile (<768px) and display only on desktop to avoid layout crowding"
  artifacts:
    - path: "index.html"
      provides: "Inline SVG mockup blocks inserted into 3 sections"
      contains: "how__visual, channels__visual, proven__visual"
    - path: "page.css"
      provides: "Layout and styling for the 3 mockup containers"
      contains: ".how__visual, .channels__visual, .proven__visual"
  key_links:
    - from: "How It Works section (.how__timeline)"
      to: ".how__visual SVG"
      via: "sibling div inserted after .how__timeline"
    - from: "Multi-Channel section (.channels__grid)"
      to: ".channels__visual SVG"
      via: "two-column wrapper wrapping both grid and visual"
    - from: "Proven section (.proven__grid)"
      to: ".proven__visual SVG"
      via: "sibling div inserted before .proven__grid"
---

<objective>
Add dashboard/UI mockup inline SVG illustrations to three sections of the Leadzly landing page: How It Works, Multi-Channel Outreach, and Proven/Results.

Purpose: These sections currently describe Leadzly's process and results in text — adding a visual mockup makes them scannable and credible to sceptical B2B decision-makers who skim pages. The hero already demonstrates this pattern works.

Output: Three new inline SVG mockup blocks embedded in index.html with supporting CSS in page.css. No new files, no external dependencies.
</objective>

<execution_context>
@$HOME/.claude/get-shit-done/workflows/execute-plan.md
</execution_context>

<context>
@.planning/PROJECT.md
@.planning/STATE.md

Brand tokens (from STATE.md):
- Green primary: #16a34a
- Green light: #22c55e
- Dark navy: #0f172a
- Green bg tint: #f0fdf4
- Font: Inter (already loaded)
- Border: use var(--color-border) and var(--color-primary)

Style reference: hero__card (white card, border, border-radius xl, shadow-lg) — replicate this container style for all three mockups.
</context>

<tasks>

<task type="auto">
  <name>Task 1: How It Works — pipeline flow mockup SVG</name>
  <files>index.html, page.css</files>
  <action>
Insert a `.how__visual` div directly after the closing `</div>` of `.how__timeline` (before `</div>` closing the `.container`). This div contains an inline SVG mockup of a 4-stage CRM pipeline board.

SVG design (600x200 viewBox, aria-hidden="true"):
- White rounded-rect card background (fill #fff, stroke var border #e2e8f0, rx 12)
- Four column headers left-to-right: "ICP Audit", "Scripting", "Live Outreach", "Meetings Booked" — each in a small rounded pill (fill #f0fdf4, text #16a34a, 11px Inter bold)
- Below each header, 2-3 small "lead row" rectangles (fill #f8fafc, stroke #e2e8f0, rx 4, 100px wide × 18px tall) stacked with 4px gap — representing leads in each stage
- The "Meetings Booked" column rows use fill #dcfce7 (green tint) and have a small checkmark circle (fill #16a34a, white check mark, 8px) on the left
- A thin top bar on the card (fill #16a34a, height 3px, rx top only) signals a branded dashboard
- Total SVG: 100% width, height auto, max 420px, centered

Add CSS in page.css under the `.how` section rules:
```css
.how__visual {
  display: none;
}
@media (min-width: 900px) {
  .how__visual {
    display: flex;
    justify-content: center;
    margin-top: var(--space-10);
  }
  .how__visual svg {
    width: 100%;
    max-width: 640px;
    height: auto;
    filter: drop-shadow(0 4px 16px rgba(0,0,0,0.08));
    border-radius: 12px;
  }
}
```
  </action>
  <verify>Open index.html in browser at ≥900px viewport — a 4-column pipeline board SVG card is visible below the 4-step timeline. Hidden at mobile.</verify>
  <done>Pipeline CRM board mockup visible below How It Works steps on desktop; hidden on mobile</done>
</task>

<task type="auto">
  <name>Task 2: Multi-Channel Outreach — outreach sequence UI mockup</name>
  <files>index.html, page.css</files>
  <action>
Wrap the existing `.channels__grid` and a new `.channels__visual` div in a `.channels__split` wrapper div. The `.channels__visual` sits to the right of the grid on desktop (text/grid left, mockup right in a 1fr/1fr grid).

SVG design (340x300 viewBox, aria-hidden="true"):
- White rounded-rect card background (rx 12, shadow token)
- Title row at top: small text "Outreach Sequence" in #0f172a 11px bold, followed by a green dot + "Active" label in #16a34a
- A vertical timeline of 5 touchpoints, each row containing:
  - Day badge on the left (small rounded rect, fill #f0fdf4, green text: "Day 1", "Day 3", "Day 5", "Day 8", "Day 12")
  - Channel icon circle (24px, fill tint based on channel: phone=#dcfce7, email=#dbeafe, linkedin=#dbeafe)
  - Channel label ("Call", "Email", "LinkedIn", "Email", "Call") in #0f172a 11px
  - Status pill on far right: first 3 show "Sent" (fill #dcfce7, text #16a34a), last 2 show "Scheduled" (fill #f1f5f9, text #64748b)
- Thin green top bar (3px, fill #16a34a)

CSS in page.css under `.channels` section rules:
```css
.channels__split {
  display: contents;
}
.channels__visual {
  display: none;
}
@media (min-width: 1024px) {
  .channels__split {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--space-12);
    align-items: center;
    margin-top: var(--space-6);
  }
  .channels__visual {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .channels__visual svg {
    width: 100%;
    max-width: 340px;
    height: auto;
    filter: drop-shadow(0 4px 20px rgba(0,0,0,0.10));
    border-radius: 12px;
  }
}
```

Note: `.channels__grid` already uses `grid-template-columns` internally — the `.channels__split` wrapper just provides the outer two-column layout on large screens. Verify `.channels__grid` grid rules still apply correctly inside the split wrapper (they will, since `.channels__grid` is a child, not replaced).
  </action>
  <verify>Open index.html in browser at ≥1024px — Multi-Channel section shows channel cards on the left and an outreach sequence timeline mockup card on the right, side by side. Hidden/stacked on mobile.</verify>
  <done>Outreach sequence UI mockup visible to the right of the channel cards on large desktop; hidden on mobile</done>
</task>

<task type="auto">
  <name>Task 3: Proven/Results — metrics dashboard mockup</name>
  <files>index.html, page.css</files>
  <action>
Insert a `.proven__visual` div directly before the `.proven__grid` div (after the `.section-header` closing div). This positions the dashboard mockup between the section header and the case study cards.

SVG design (580x180 viewBox, aria-hidden="true"):
- White rounded-rect card background (rx 12)
- Green top bar (3px, fill #16a34a)
- Header row: "Campaign Dashboard" label (11px, #0f172a bold) + "This Month" badge (fill #f0fdf4, text #16a34a, 10px)
- Four metric cards side by side, each a rounded rect (fill #f8fafc, stroke #e2e8f0, rx 8, ~120px wide × 80px tall):
  - Card 1: "47" in 24px bold #0f172a, label "Meetings Booked" in 10px #64748b, small green up-arrow
  - Card 2: "£94k" in 24px bold #0f172a, label "Pipeline Value" in 10px #64748b, small green up-arrow
  - Card 3: "12" in 24px bold #0f172a, label "Industries" in 10px #64748b
  - Card 4: "94%" in 24px bold #16a34a, label "Show Rate" in 10px #64748b, small green up-arrow
- Below the metric cards: a simple 5-bar mini bar chart (bars in #16a34a at varying heights ~20-50px, x-labels Mon–Fri in 9px #94a3b8) filling the remaining card height

CSS in page.css under `.proven` section rules:
```css
.proven__visual {
  display: none;
}
@media (min-width: 768px) {
  .proven__visual {
    display: flex;
    justify-content: center;
    margin-bottom: var(--space-8);
  }
  .proven__visual svg {
    width: 100%;
    max-width: 640px;
    height: auto;
    filter: drop-shadow(0 4px 16px rgba(0,0,0,0.08));
    border-radius: 12px;
  }
}
```
  </action>
  <verify>Open index.html in browser at ≥768px — Proven/Results section shows a 4-metric dashboard card above the case study grid. Hidden on mobile.</verify>
  <done>Metrics dashboard SVG visible above the case study cards on tablet and desktop; hidden on mobile</done>
</task>

<task type="checkpoint:human-verify" gate="blocking">
  <what-built>Three inline SVG mockup illustrations added to How It Works (pipeline board), Multi-Channel Outreach (sequence UI), and Proven/Results (metrics dashboard). All hidden on mobile, visible on desktop.</what-built>
  <how-to-verify>
    1. Open index.html in a browser
    2. At viewport ≥1280px: scroll to How It Works — confirm a 4-column CRM pipeline board SVG is visible below the steps
    3. Scroll to Multi-Channel Outreach — confirm an outreach sequence timeline card appears to the right of the 4 channel cards
    4. Scroll to Proven/Results — confirm a 4-metric dashboard card appears above the case study grid
    5. Resize to 375px (mobile) — confirm all three SVGs are hidden, layouts look correct
    6. Check that brand colours (green, white, navy) are consistent with the rest of the page
  </how-to-verify>
  <resume-signal>Type "approved" if the visuals look correct, or describe any issues to fix</resume-signal>
</task>

</tasks>

<verification>
- Three SVG mockup blocks exist in index.html inside `.how__visual`, `.channels__visual`, `.proven__visual`
- All three are hidden below their respective breakpoints (mobile-first display:none)
- No new external images or fonts referenced
- Hero section (`.hero__visual`) is untouched
- Brand colours match: #16a34a, #0f172a, #f0fdf4 used throughout mockups
</verification>

<success_criteria>
- How It Works has a CRM pipeline board mockup visible at ≥900px
- Multi-Channel Outreach has an outreach sequence UI mockup visible at ≥1024px (side by side with channel cards)
- Proven/Results has a metrics dashboard mockup visible at ≥768px (above case study cards)
- All three mockups hidden on mobile
- No regressions in existing section layouts
- User confirms visual quality via checkpoint
</success_criteria>

<output>
After completion, create `.planning/quick/260428-hbj-add-graphics-to-the-site-to-bring-it-to-/260428-hbj-SUMMARY.md` documenting what was built, SVG design decisions, and any layout choices made.
</output>
