---
phase: 01-foundation-hygiene
plan: 01
type: execute
wave: 1
depends_on: []
files_modified:
  - index.html
autonomous: false
requirements:
  - NAV-01
  - NAV-02
  - NAV-03
  - QWIN-01
  - QWIN-02

must_haves:
  truths:
    - "Sharing leadzly.co on Slack/LinkedIn renders OG image (the logo-with-text PNG)"
    - "Browser tab shows the Leadzly logo icon as a favicon"
    - "Nav is sticky with blur and scroll-shadow — no code change required, already live"
    - "Hamburger menu opens/closes on mobile — already live"
    - "All external links carry rel=noopener noreferrer — already verified in code"
  artifacts:
    - path: "index.html"
      provides: "og:image, og:url, og:image:type, og:image:alt, SVG favicon link, PNG favicon link"
      contains: "og:image"
  key_links:
    - from: "index.html <head>"
      to: "https://leadzly.co/Leadzly-logo_with-text.png"
      via: "meta property=og:image"
      pattern: "og:image.*Leadzly-logo_with-text\\.png"
    - from: "index.html <head>"
      to: "Leadzly-logo_without-text.svg"
      via: "link rel=icon type=image/svg+xml"
      pattern: "rel=\"icon\".*svg"
---

<objective>
Add the four missing Open Graph tags and two favicon link elements to index.html's head section, and confirm the three pre-implemented navigation requirements (NAV-01, NAV-02, NAV-03) are present in the existing code.

Purpose: Correct OG metadata ensures the page unfurls correctly when shared on LinkedIn, WhatsApp, or Slack. Favicon adds brand recognition in the browser tab. Both are zero-risk head-only changes that must happen before any structural HTML edits.
Output: index.html with complete OG tags and favicon links; checkpoint confirming NAV requirements are met.
</objective>

<execution_context>
@$HOME/.claude/get-shit-done/workflows/execute-plan.md
@$HOME/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@.planning/PROJECT.md
@.planning/ROADMAP.md
@.planning/phases/01-foundation-hygiene/01-CONTEXT.md
</context>

<tasks>

<task type="auto" id="task-1">
  <name>Task 1: Add missing OG tags and favicon links to index.html head</name>
  <files>index.html</files>

  <read_first>
    - index.html — read the entire file before touching it; the existing head already contains og:title, og:description, og:type. You must insert AFTER og:type and not duplicate existing tags.
    - .planning/phases/01-foundation-hygiene/01-CONTEXT.md — decisions D-01 through D-07 define exact values.
  </read_first>

  <action>
Open index.html. Locate the existing Open Graph block in head (lines 27–29). After the existing `<meta property="og:type" content="website">` line, insert exactly these four tags on new lines:

```html
  <meta property="og:image" content="https://leadzly.co/Leadzly-logo_with-text.png">
  <meta property="og:image:type" content="image/png">
  <meta property="og:image:alt" content="Leadzly logo">
  <meta property="og:url" content="https://leadzly.co/">
```

Then locate the CSS link block (the three `<link rel="stylesheet">` lines). BEFORE `<link rel="stylesheet" href="./base.css">`, insert these two favicon links on new lines:

```html
  <link rel="icon" type="image/svg+xml" href="./Leadzly-logo_without-text.svg">
  <link rel="icon" type="image/png" sizes="32x32" href="./Leadzly-logo_without-text.png">
```

Do NOT add apple-touch-icon (per D-03). Do NOT change any existing tags. Do NOT add a canonical link or any other meta tag not listed here.

After editing, the head section OG block should read (in order):
1. og:title
2. og:description
3. og:type
4. og:image  ← new
5. og:image:type  ← new
6. og:image:alt  ← new
7. og:url  ← new

And the link block should read (in order):
1. link rel="icon" type="image/svg+xml"  ← new
2. link rel="icon" type="image/png" sizes="32x32"  ← new
3. link rel="preconnect" (fonts)
4. link rel="preconnect" (fonts gstatic)
5. link href (Google Fonts)
6. link rel="stylesheet" base.css
7. link rel="stylesheet" style.css
8. link rel="stylesheet" page.css
  </action>

  <verify>
    <automated>grep -n "og:image\|og:url\|og:image:type\|og:image:alt\|rel=\"icon\"" "index.html"</automated>
  </verify>

  <acceptance_criteria>
    - index.html contains: `property="og:image" content="https://leadzly.co/Leadzly-logo_with-text.png"`
    - index.html contains: `property="og:image:type" content="image/png"`
    - index.html contains: `property="og:image:alt" content="Leadzly logo"`
    - index.html contains: `property="og:url" content="https://leadzly.co/"`
    - index.html contains: `rel="icon" type="image/svg+xml" href="./Leadzly-logo_without-text.svg"`
    - index.html contains: `rel="icon" type="image/png" sizes="32x32" href="./Leadzly-logo_without-text.png"`
    - Existing tags og:title, og:description, og:type are still present and unchanged
    - No apple-touch-icon link exists
    - The grep command above returns exactly 6 matching lines
  </acceptance_criteria>

  <done>index.html head contains all six new elements; existing tags are untouched; file opens in browser without errors.</done>
</task>

<task type="checkpoint:human-verify" gate="blocking">
  <name>Task 2: Verify NAV-01, NAV-02, NAV-03 and OG/favicon in browser</name>

  <read_first>
    - index.html — confirm code is in place before asking user to verify visually
    - .planning/phases/01-foundation-hygiene/01-CONTEXT.md — decisions D-00a, D-00b, D-00c describe what "already implemented" means
  </read_first>

  <what-built>
    Task 1 added og:image, og:image:type, og:image:alt, og:url, and two favicon link elements to index.html head. The NAV requirements (NAV-01, NAV-02, NAV-03) were confirmed already implemented in the existing code — no code changes were needed for those.
  </what-built>

  <how-to-verify>
    Open index.html in a local browser (double-click or use a local dev server):

    1. **Favicon (QWIN-02):** Look at the browser tab — the Leadzly logo (green funnel icon) should appear as the favicon. If using Chrome, also check bookmarks bar after bookmarking.

    2. **Sticky nav with blur/shadow (NAV-01):** Scroll down more than 50px — the nav should remain fixed at the top and gain a visible shadow. Scroll back to top — shadow should disappear.

    3. **Hamburger menu (NAV-02):** Resize the browser to a mobile width (~375px). A three-line hamburger button should appear in the top-right. Clicking it should open the nav links. Clicking again (or clicking a link) should close it.

    4. **External links (NAV-03):** Right-click the "Book a Strategy Call" nav CTA, click "Inspect" — confirm the anchor has `rel="noopener noreferrer"`.

    5. **OG tags (QWIN-01):** Open browser DevTools → Elements → search `<head>` for `og:image` — confirm `content="https://leadzly.co/Leadzly-logo_with-text.png"` is present.
  </how-to-verify>

  <resume-signal>Type "approved" if all 5 checks pass. If any check fails, describe which check failed and what you saw instead.</resume-signal>
</task>

</tasks>

<verification>
Run after Task 1 completes:

```bash
grep -c "og:image\|og:url\|og:image:type\|og:image:alt\|rel=\"icon\"" index.html
```
Expected output: 6 (one line per new element)

```bash
grep "og:title\|og:description\|og:type" index.html
```
Expected output: 3 lines confirming existing tags are still present.
</verification>

<success_criteria>
- index.html head contains exactly the 4 new OG tags and 2 favicon link tags specified in decisions D-03 through D-07
- Existing OG tags (og:title, og:description, og:type) are present and unchanged
- Browser tab displays the Leadzly logo as favicon (confirmed by human checkpoint)
- NAV-01, NAV-02, NAV-03 confirmed working in browser (confirmed by human checkpoint)
</success_criteria>

<output>
After completion, create `.planning/phases/01-foundation-hygiene/01-01-SUMMARY.md` following the summary template.
</output>
