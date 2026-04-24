---
phase: 01-foundation-hygiene
plan: 02
type: execute
wave: 2
depends_on:
  - 01-PLAN-meta-favicon
files_modified:
  - index.html
autonomous: false
requirements:
  - QWIN-04

must_haves:
  truths:
    - "First-time visitor sees a GDPR cookie consent banner at the bottom of the viewport"
    - "Clicking Accept dismisses the banner and it never reappears on future visits"
    - "GA4 does NOT fire on first visit until the user clicks Accept"
    - "GA4 fires immediately when Accept is clicked in the same session"
    - "Returning visitors (consent stored) see no banner and GA4 fires normally on page load"
  artifacts:
    - path: "index.html"
      provides: "Cookie banner HTML element + banner CSS + banner/GA4-gating JS"
      contains: "leadzly_cookie_consent"
  key_links:
    - from: "cookie banner Accept button"
      to: "localStorage.setItem('leadzly_cookie_consent', 'accepted')"
      via: "click event handler"
      pattern: "leadzly_cookie_consent.*accepted"
    - from: "GA4 gtag init block"
      to: "localStorage.getItem('leadzly_cookie_consent')"
      via: "consent check before calling gtag config"
      pattern: "leadzly_cookie_consent"
---

<objective>
Add a GDPR-compliant cookie consent banner to index.html: fixed full-width bar at viewport bottom, Accept-only controls, localStorage persistence, and real GA4 gating (GA4 does not fire until the user accepts).

Purpose: GDPR compliance for UK/EU visitors. Also improves trust — sceptical B2B decision-makers notice when a site does not have a consent mechanism. GA4 gating is genuine, not cosmetic.
Output: A styled banner that appears on first visit, persists consent in localStorage, and gates GA4 activation on that consent.
</objective>

<execution_context>
@$HOME/.claude/get-shit-done/workflows/execute-plan.md
@$HOME/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@.planning/phases/01-foundation-hygiene/01-CONTEXT.md
@.planning/phases/01-foundation-hygiene/01-01-SUMMARY.md
</context>

<tasks>

<task type="auto" id="task-1">
  <name>Task 1: Add cookie banner HTML and CSS to index.html</name>
  <files>index.html</files>

  <read_first>
    - index.html — read the full file before editing; understand where the closing </body> tag is and what the existing inline `<script>` block looks like. The banner HTML goes just before </body>; CSS goes inside a new `<style>` block in `<head>`.
    - style.css — read lines 1–100 to confirm exact CSS custom property names (--color-dark, --color-primary, --color-text-inverse, --radius-md, --space-3, --space-4, --space-6, --font-body, --text-sm, --transition-interactive, --shadow-lg) before using them.
    - .planning/phases/01-foundation-hygiene/01-CONTEXT.md — decisions D-08 through D-13 define exact styling and behaviour.
  </read_first>

  <action>
**Step A — Add cookie banner CSS.**

In index.html, inside `<head>`, after the three `<link rel="stylesheet">` lines, add a new `<style>` block containing:

```css
<style>
/* ===== GDPR Cookie Banner ===== */
.cookie-banner {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 9999;
  background: var(--color-dark);
  color: var(--color-text-inverse);
  padding: var(--space-4) var(--space-6);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-4);
  box-shadow: var(--shadow-lg);
  transform: translateY(100%);
  transition: transform 300ms cubic-bezier(0.16, 1, 0.3, 1);
  flex-wrap: wrap;
}
.cookie-banner--visible {
  transform: translateY(0);
}
.cookie-banner__text {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  color: var(--color-text-inverse);
  margin: 0;
  flex: 1 1 auto;
  min-width: 200px;
}
.cookie-banner__accept {
  flex-shrink: 0;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 600;
  background: var(--color-primary);
  color: var(--color-text-inverse);
  border: none;
  border-radius: var(--radius-md);
  padding: var(--space-3) var(--space-6);
  cursor: pointer;
  transition: background var(--transition-interactive),
              transform var(--transition-interactive);
  white-space: nowrap;
}
.cookie-banner__accept:hover {
  background: var(--color-primary-hover);
  transform: translateY(-1px);
}
.cookie-banner__accept:active {
  transform: translateY(0);
}
@media (max-width: 480px) {
  .cookie-banner {
    flex-direction: column;
    align-items: flex-start;
    padding: var(--space-4);
  }
  .cookie-banner__accept {
    width: 100%;
  }
}
</style>
```

**Step B — Add cookie banner HTML.**

Just before the closing `</body>` tag (and BEFORE the existing `<script>` block), insert:

```html
  <!-- ===== GDPR Cookie Banner ===== -->
  <div class="cookie-banner" id="cookie-banner" role="dialog" aria-live="polite" aria-label="Cookie consent" hidden>
    <p class="cookie-banner__text">We use cookies to understand how visitors use our site and to improve your experience.</p>
    <button class="cookie-banner__accept" id="cookie-accept">Accept</button>
  </div>
```

Note: The banner starts with `hidden` attribute and `transform: translateY(100%)`. The JS (Task 2) removes `hidden` and adds `.cookie-banner--visible` after a short delay to trigger the slide-up animation.

Do NOT add a privacy policy link (per D-12). Do NOT add a Decline button (per D-09).
  </action>

  <verify>
    <automated>grep -n "cookie-banner\|leadzly_cookie_consent\|cookie-accept" "index.html"</automated>
  </verify>

  <acceptance_criteria>
    - index.html contains `class="cookie-banner"` with `id="cookie-banner"`
    - index.html contains `id="cookie-accept"` on the Accept button
    - index.html contains `hidden` attribute on the cookie-banner div
    - index.html contains `.cookie-banner--visible` in the CSS style block
    - index.html contains `background: var(--color-dark)` in the cookie-banner CSS rule
    - index.html contains `background: var(--color-primary)` in the cookie-banner__accept CSS rule
    - The `<style>` block for the banner is inside `<head>`, not inline on the element
    - No Decline button or privacy policy link exists in the banner HTML
  </acceptance_criteria>

  <done>Banner HTML and CSS present in index.html; no visual regressions on existing sections (the banner starts hidden so it does not affect layout).</done>
</task>

<task type="auto" id="task-2">
  <name>Task 2: Add banner JS and GA4 consent gating to index.html script block</name>
  <files>index.html</files>

  <read_first>
    - index.html — read the full file again after Task 1's edits. Specifically study:
      (a) The existing Partytown/GA4 setup in `<head>` (lines 4–22) — understand the `type="text/partytown"` script tags and the duplicate `window.dataLayer` / `gtag("config")` call.
      (b) The existing inline `<script>` block at the bottom of `<body>` — the new cookie IIFE goes at the END of this block, after all existing IIFEs.
    - .planning/phases/01-foundation-hygiene/01-CONTEXT.md — decisions D-08 through D-13 and the Claude's Discretion section on GA4 immediate firing.
  </read_first>

  <action>
**GA4 gating strategy:**

The existing `<head>` contains a non-Partytown `<script>` block (lines 17–22 in the original file) that calls `gtag("config", "G-RHWLZ0HHLV")` unconditionally. This must be GATED on consent.

**Step A — Gate the non-Partytown GA4 config call.**

In `<head>`, find the plain (non-partytown) script block:
```html
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag("js", new Date());
    gtag("config", "G-RHWLZ0HHLV");
  </script>
```

Replace it with a consent-aware version:
```html
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag("js", new Date());
    // GA4 config only fires if consent was already given (returning visitor)
    if (localStorage.getItem('leadzly_cookie_consent') === 'accepted') {
      gtag("config", "G-RHWLZ0HHLV");
    }
  </script>
```

This means: returning visitors who already accepted get GA4 fired on page load; first-time visitors do not.

**Step B — Add cookie banner IIFE to the existing script block.**

Inside the existing inline `<script>` block at the bottom of `<body>`, append a new IIFE as the last item (after all existing IIFEs, before the closing `</script>` tag):

```javascript
    // GDPR Cookie Banner
    (function() {
      var CONSENT_KEY = 'leadzly_cookie_consent';
      var banner = document.getElementById('cookie-banner');
      var acceptBtn = document.getElementById('cookie-accept');

      if (!banner || !acceptBtn) return;

      // If already accepted: keep banner hidden, GA4 already fired in <head>
      if (localStorage.getItem(CONSENT_KEY) === 'accepted') return;

      // First visit: reveal banner with slide-up animation
      banner.removeAttribute('hidden');
      // rAF ensures the hidden removal paints before adding the transition class
      requestAnimationFrame(function() {
        requestAnimationFrame(function() {
          banner.classList.add('cookie-banner--visible');
        });
      });

      acceptBtn.addEventListener('click', function() {
        // Persist consent
        localStorage.setItem(CONSENT_KEY, 'accepted');

        // Fire GA4 immediately for this session (per Claude's Discretion)
        if (typeof gtag === 'function') {
          gtag("config", "G-RHWLZ0HHLV");
        }

        // Slide banner down and remove from DOM after animation
        banner.classList.remove('cookie-banner--visible');
        banner.addEventListener('transitionend', function() {
          banner.setAttribute('hidden', '');
        }, { once: true });
      });
    })();
```

Do NOT create a new `<script>` tag — append this IIFE inside the EXISTING script block. Do NOT remove or alter the Partytown scripts — they run in a web worker and are separate from the consent mechanism. Do NOT alter any other existing IIFE.
  </action>

  <verify>
    <automated>grep -n "leadzly_cookie_consent\|cookie-banner--visible\|gtag.*config.*RHWLZ\|cookie-accept" "index.html"</automated>
  </verify>

  <acceptance_criteria>
    - index.html contains `localStorage.getItem('leadzly_cookie_consent') === 'accepted'` in the head script block (the GA4 gate on page load)
    - index.html contains `localStorage.setItem(CONSENT_KEY, 'accepted')` in the Accept click handler
    - index.html contains `gtag("config", "G-RHWLZ0HHLV")` inside the Accept click handler (for immediate firing)
    - index.html contains `cookie-banner--visible` in both the CSS (Task 1) and the JS (Task 2)
    - index.html contains `removeAttribute('hidden')` on the banner reveal path
    - The original Partytown `type="text/partytown"` script tags are still present and unchanged
    - There is only ONE inline `<script>` block at the bottom of `<body>` (new IIFE appended inside it, not a second script tag)
  </acceptance_criteria>

  <done>GA4 is gated on consent — does not fire for new visitors until Accept is clicked. Banner appears on first visit, disappears after Accept, and does not reappear on subsequent visits.</done>
</task>

<task type="checkpoint:human-verify" gate="blocking">
  <name>Task 3: Verify GDPR banner behaviour in browser</name>

  <read_first>
    - index.html — confirm both Tasks 1 and 2 changes are present before asking user to test.
  </read_first>

  <what-built>
    Tasks 1 and 2 added the full GDPR cookie consent system: a dark navy bottom bar with green Accept button, localStorage persistence using key `leadzly_cookie_consent`, and real GA4 gating (GA4 config call blocked until consent).
  </what-built>

  <how-to-verify>
    Open index.html in a browser, then follow these steps in order:

    **Test 1 — First visit (banner appears):**
    1. Open DevTools → Application → Local Storage → clear any existing `leadzly_cookie_consent` entry.
    2. Hard-refresh the page (Ctrl+Shift+R or Cmd+Shift+R).
    3. Expected: A dark navy bar slides up from the bottom of the viewport within ~300ms. It should say "We use cookies to understand how visitors use our site and to improve your experience." with a green "Accept" button.

    **Test 2 — GA4 blocked on first visit:**
    4. While the banner is visible (before clicking Accept), open DevTools → Network tab → filter by "googletagmanager" or "collect".
    5. Expected: No GA4 network requests visible (gtag config has not fired).

    **Test 3 — Accept dismisses banner:**
    6. Click the green "Accept" button.
    7. Expected: The banner slides down and disappears. DevTools → Application → Local Storage should now show `leadzly_cookie_consent = accepted`.

    **Test 4 — GA4 fires immediately on Accept:**
    8. In the Network tab, after clicking Accept, check for a request to `googletagmanager.com` or a `/collect` pixel.
    9. Expected: A GA4 network request fires within ~1 second of clicking Accept (may appear as a request to `www.google-analytics.com/g/collect`).

    **Test 5 — Banner does not reappear:**
    10. Refresh the page (normal F5 refresh, NOT hard refresh).
    11. Expected: The banner does NOT appear. The page loads cleanly.

    **Test 6 — Banner styling:**
    12. Confirm the banner background is dark navy, text is white, and the Accept button is green — matching the Leadzly brand.
    13. On a mobile-width viewport (~375px): confirm the text and button stack vertically and the button is full-width.
  </how-to-verify>

  <resume-signal>Type "approved" if all 6 tests pass. If any test fails, describe which test number failed and what you observed instead.</resume-signal>
</task>

</tasks>

<verification>
After Tasks 1 and 2, run:

```bash
grep -c "leadzly_cookie_consent" index.html
```
Expected: 4 or more occurrences (head gate check, IIFE key declaration, setItem call, getItem check in IIFE).

```bash
grep "type=\"text/partytown\"" index.html
```
Expected: 2 lines — confirming the Partytown scripts are untouched.
</verification>

<success_criteria>
- GDPR banner appears on first visit and slides up from the bottom (dark navy, white text, green Accept button)
- Clicking Accept stores `leadzly_cookie_consent = accepted` in localStorage and fires gtag config immediately
- Banner does not appear on subsequent page loads once accepted
- GA4 does not fire before consent is given (verified via Network tab)
- All existing page functionality unchanged (nav, scroll animations, smooth scroll, theme toggle all still work)
- Human checkpoint approved (Task 3)
</success_criteria>

<output>
After completion, create `.planning/phases/01-foundation-hygiene/01-02-SUMMARY.md` following the summary template.
</output>
