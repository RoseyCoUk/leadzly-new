# Phase 8: Structured Data — Research

**Researched:** 2026-04-28
**Domain:** JSON-LD structured data (Organization, LocalBusiness, FAQPage, Service schema types)
**Confidence:** HIGH

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| SCHEMA-01 | Organization + LocalBusiness JSON-LD block in `<head>` (name, URL, logo, contactPoint, areaServed UK/NI) | Organization and LocalBusiness schema documented below; all required property values sourced directly from index.html and STATE.md |
| SCHEMA-02 | FAQPage JSON-LD block mapping all 7 existing FAQ questions and answers | All 7 Q&A pairs extracted verbatim from index.html FAQ section; FAQPage schema structure documented below |
| SCHEMA-03 | Service JSON-LD block covering telesales, meeting booking, and BDM provision offerings | All three service names and descriptions sourced from services section in index.html; Service schema structure documented below |

</phase_requirements>

---

## Summary

Phase 8 adds three JSON-LD script blocks to the `<head>` of index.html. This is a pure HTML edit — no CSS, no JS, no new files. Each block uses the `application/ld+json` script type and targets a different Google rich result: the business knowledge panel (Organization + LocalBusiness), FAQ rich snippets (FAQPage), and service entity understanding (Service).

The FAQ rich result eligibility note from Google is important: as of current documentation, FAQ rich results are restricted to "well-known, authoritative" government and health-focused sites. For a B2B sales agency, the FAQPage schema still benefits crawlers and knowledge graph understanding even if the visual FAQ accordion snippet does not appear in SERPs immediately. The schema is valid and worth implementing regardless.

All data values for the three blocks already exist in index.html. No new content needs to be written or invented — this is purely an encoding task: translating visible page content into machine-readable JSON-LD.

**Primary recommendation:** Write three separate `<script type="application/ld+json">` blocks, one per schema type, placed consecutively in `<head>` before the closing `</head>` tag. Do not combine into a `@graph` — separate blocks are easier to validate individually and produce cleaner Rich Results Test output.

---

## Project Constraints (from CLAUDE.md)

- **Tech stack:** HTML/CSS/vanilla JS only — no build system, no npm, no frameworks
- **Fonts:** Inter already loaded — no new font imports
- **Logo assets:** Use existing SVG/PNG files only
- **No backend:** All logic client-side
- **JSON-LD blocks:** Must go in `<head>` before closing `</head>` tag (confirmed in STATE.md v1.1 note)

---

## Standard Stack

### Core

| Library / Tool | Version | Purpose | Why Standard |
|----------------|---------|---------|--------------|
| JSON-LD | — | Machine-readable schema embedded in HTML | Google's preferred structured data format; no external dependency |
| schema.org vocabulary | — | Defines type names and property names | Universal; validated by Google and schema.org validator |

### Supporting

| Tool | Purpose | When to Use |
|------|---------|-------------|
| Google Rich Results Test | Validate schema is eligible for rich results | After writing each block; paste URL or code snippet |
| schema.org Validator | Zero-error validation before commit | Cross-check after Rich Results Test |

### Alternatives Considered

| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Separate `<script>` blocks | Single `@graph` block | `@graph` is valid but harder to debug per-type; separate blocks pass identical validation and are clearer to review |
| JSON-LD | Microdata or RDFa | JSON-LD is Google's stated preference for new implementations; microdata requires HTML attribute changes across the DOM |

**Installation:** None required. JSON-LD is a `<script>` tag with no external dependency.

---

## Architecture Patterns

### Recommended Structure

Three `<script type="application/ld+json">` blocks appended to `<head>` in this order:

1. Organization + LocalBusiness (SCHEMA-01)
2. FAQPage (SCHEMA-02)
3. Service — one block, three `@graph` items or three separate blocks (SCHEMA-03)

### Pattern 1: Organization + LocalBusiness Combined

Google recommends using the most specific type. For Leadzly, `LocalBusiness` is the correct primary type (location-specific B2B service business). Organization properties (logo, sameAs, contactPoint) can be added directly to the LocalBusiness type since LocalBusiness extends Organization in the schema.org hierarchy.

```json
{
  "@context": "https://schema.org",
  "@type": ["LocalBusiness", "ProfessionalService"],
  "name": "Leadzly",
  "url": "https://leadzly.co/",
  "logo": "https://leadzly.co/Leadzly-logo_with-text.png",
  "description": "B2B sales outsourcing agency based in Northern Ireland. We book 20+ qualified meetings per month through dedicated telesales, appointment booking, and BDM provision.",
  "areaServed": [
    { "@type": "AdministrativeArea", "name": "United Kingdom" },
    { "@type": "AdministrativeArea", "name": "Northern Ireland" }
  ],
  "contactPoint": {
    "@type": "ContactPoint",
    "contactType": "sales",
    "email": "allan.chan@elevateoco.com",
    "availableLanguage": "English"
  },
  "address": {
    "@type": "PostalAddress",
    "addressRegion": "Northern Ireland",
    "addressCountry": "GB"
  },
  "parentOrganization": {
    "@type": "Organization",
    "name": "Elevateo Co"
  }
}
```

**Note on contactPoint:** The page does not display a phone number or email address in the visible UI. The STATE.md Calendly URL (`https://calendly.com/allan-chan-leadzly`) is a booking link, not a contact number. For `contactType`, use `"sales"` since the Calendly CTA is for sales calls. Email should only be included if it appears on the visible page — if it does not, omit `email` from contactPoint to avoid schema-content mismatch errors. **The planner must verify what contact information is actually visible on the page before writing the contactPoint block.**

### Pattern 2: FAQPage

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "How quickly can you start generating meetings?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "We typically onboard new clients within 30 days. Week one is ICP audit and script development; week two is outreach infrastructure setup; week three is live calling. Most clients see their first qualified meetings in weeks 3–4."
      }
    }
    ...
  ]
}
```

All 7 FAQ pairs are documented in the **Extracted Content** section below. The `text` value in `acceptedAnswer` must match the visible answer text — HTML entities (e.g., `&ndash;`, `&rsquo;`) must be decoded to their Unicode equivalents in the JSON string.

### Pattern 3: Service (three offerings)

Use a single `<script>` block with a `@graph` array containing three Service nodes:

```json
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Service",
      "name": "Telesales Outsourcing",
      "description": "Professional cold callers who know your ICP inside out. We handle outbound calls, objection handling, and warm handoffs — so your calendar fills itself.",
      "provider": { "@type": "LocalBusiness", "name": "Leadzly" },
      "serviceType": "B2B Telesales",
      "areaServed": "United Kingdom"
    },
    {
      "@type": "Service",
      "name": "Meeting Booking",
      "description": "20+ qualified appointments per month with decision-makers in your target market. No tire-kickers, no wasted time.",
      "provider": { "@type": "LocalBusiness", "name": "Leadzly" },
      "serviceType": "B2B Appointment Setting",
      "areaServed": "United Kingdom"
    },
    {
      "@type": "Service",
      "name": "BDM Provision",
      "description": "Dedicated Business Development Managers embedded in your sales process. Full-time focus, fractional cost.",
      "provider": { "@type": "LocalBusiness", "name": "Leadzly" },
      "serviceType": "Business Development Management",
      "areaServed": "United Kingdom"
    }
  ]
}
```

### Anti-Patterns to Avoid

- **Hallucinating contact details:** Do not add a phone number, street address, or email to the schema if it does not appear visibly on the page. Google's validator flags schema-to-content mismatches.
- **HTML entities in JSON:** JSON-LD is not HTML. The string `"weeks 3&ndash;4"` is wrong; use `"weeks 3–4"` (Unicode en-dash).
- **Unclosed quotes in JSON:** A single typo renders the entire block invalid. Validate with the schema.org validator before committing.
- **Placing blocks in `<body>`:** The requirement specifies `<head>`. Google accepts both locations, but the project constraint requires `<head>`.
- **Combining all three into one @graph without testing separately:** Validation errors in one block can mask errors in another when combined. Write and test each block individually first.

---

## Extracted Content (Verbatim from index.html)

### Business Data (for SCHEMA-01)

| Property | Value |
|----------|-------|
| Name | Leadzly |
| URL | `https://leadzly.co/` |
| Logo (PNG) | `https://leadzly.co/Leadzly-logo_with-text.png` |
| Logo (SVG) | `https://leadzly.co/Leadzly-logo_with-text.svg` |
| Description | "Leadzly is a division of Elevateo Co, headquartered in Northern Ireland." |
| Parent org | Elevateo Co |
| Area served | UK & Ireland (hero eyebrow text); Northern Ireland (about section) |
| Calendly booking URL | `https://calendly.com/allan-chan-leadzly` |
| Visible contact info | None — no phone or email displayed in visible page UI |

### FAQ Pairs (for SCHEMA-02)

All 7 questions and answers, decoded from HTML entities:

| # | Question | Answer (decoded) |
|---|----------|-----------------|
| 1 | How quickly can you start generating meetings? | We typically onboard new clients within 30 days. Week one is ICP audit and script development; week two is outreach infrastructure setup; week three is live calling. Most clients see their first qualified meetings in weeks 3–4. |
| 2 | What does "qualified meeting" actually mean? | A qualified meeting is a booked appointment with a decision-maker who fits your ICP, understands what you do, and has agreed to the call. We don't count no-shows or rescheduled meetings that never happen. |
| 3 | Do I have to sign a long-term contract? | No. All plans are month-to-month. We don't believe in locking clients in — if we're not delivering, you should be free to leave. Our 30-day performance guarantee reflects that confidence. |
| 4 | What industries do you work with? | We've run campaigns across SaaS, recruitment, e-commerce, financial services, professional services, construction, managed IT, and more. If your product or service is sold B2B, we can build a campaign for it. |
| 5 | How is Leadzly different from a typical cold-calling agency? | Most agencies operate offshore and optimise for call volume. We're a UK-based team that optimises for meeting quality — every lead is qualified before it hits your calendar. You pay for results, not activity. |
| 6 | What information do you need to get started? | We need your ICP (ideal customer profile), your core value proposition, and access to your calendar or CRM. We handle the rest — list building, scripting, outreach, and qualification. |
| 7 | What happens if you don't hit the meeting targets? | Our 30-day guarantee means if we don't deliver qualified meetings in your first month, you don't pay for that month. We stand behind our results. |

### Service Descriptions (for SCHEMA-03)

| Service | Description (from page) |
|---------|------------------------|
| Telesales Outsourcing | Professional cold callers who know your ICP inside out. We handle outbound calls, objection handling, and warm handoffs — so your calendar fills itself. |
| Meeting Booking | 20+ qualified appointments per month with decision-makers in your target market. No tire-kickers, no wasted time. |
| BDM Provision | Dedicated Business Development Managers embedded in your sales process. Full-time focus, fractional cost. |

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Structured data format | Custom meta tags or microdata | JSON-LD script blocks | Google's preferred format; no DOM changes required |
| Validation | Manual inspection | Google Rich Results Test + schema.org Validator | Catches property-level errors and eligibility issues automatically |
| Schema vocabulary | Invented property names | schema.org standard types and properties | Non-standard properties are ignored by all parsers |

---

## Common Pitfalls

### Pitfall 1: HTML Entity Encoding in JSON Strings
**What goes wrong:** HTML entities like `&ndash;`, `&rsquo;`, `&ldquo;` copied from HTML source appear as literal strings in JSON, which schema validators reject or misread.
**Why it happens:** Copy-paste from HTML source without decoding.
**How to avoid:** Decode all HTML entities to Unicode before writing JSON values. `&ndash;` → `–`, `&rsquo;` → `'`, `&ldquo;` → `"`, `&rdquo;` → `"`, `&mdash;` → `—`.
**Warning signs:** Validator shows unexpected characters in string values.

### Pitfall 2: Schema-Content Mismatch
**What goes wrong:** Schema includes properties (phone number, email, street address) that do not appear in the visible page content.
**Why it happens:** Temptation to "complete" the schema with data that isn't on the page.
**How to avoid:** Every value in the JSON-LD must correspond to content visible to the user on the page. For Leadzly, there is no visible phone number, street address, or email — do not add them.
**Warning signs:** Google Search Console reports "Item not eligible" or "Missing field" contradictions.

### Pitfall 3: Invalid JSON Syntax
**What goes wrong:** A trailing comma, unclosed bracket, or mismatched quote renders the entire `<script>` block silently invalid.
**Why it happens:** Manual JSON authoring without a linter.
**How to avoid:** Paste the completed JSON into the schema.org validator (https://validator.schema.org/) before committing. The validator reports line-level JSON parse errors.
**Warning signs:** Rich Results Test shows "No items detected" even though the script tag is present in head.

### Pitfall 4: Wrong Script Type Attribute
**What goes wrong:** Using `<script>` without `type="application/ld+json"` causes the browser to execute the JSON as JavaScript, throwing a syntax error.
**How to avoid:** Always write `<script type="application/ld+json">`.

### Pitfall 5: FAQPage Rich Result Eligibility
**What goes wrong:** FAQPage schema is implemented correctly but Google does not display FAQ accordion snippets in SERPs.
**Why it happens:** As of 2025, Google restricts FAQ rich results to authoritative government and health sites. A B2B sales agency does not qualify for the visual SERP feature.
**How to avoid:** Implement the schema anyway — it still signals content to crawlers and can contribute to knowledge graph understanding. Do not skip SCHEMA-02 because of this; the requirement is zero validation errors in the schema.org validator, not a live SERP appearance.
**Warning signs:** This is expected behaviour, not an error.

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Microdata in HTML attributes | JSON-LD in `<script>` tag | ~2016–2018 | No DOM changes required; easier to maintain |
| One `@type` only | Multiple `@type` as array e.g. `["LocalBusiness","ProfessionalService"]` | Ongoing | More specific classification |
| `contactPoint` with phone required | `contactPoint` with only `contactType` and `availableLanguage` acceptable | — | More flexible for businesses without a public phone number |

---

## Validation Architecture

### Test Framework

| Property | Value |
|----------|-------|
| Framework | Manual validation via external tools (no automated test framework — static HTML project) |
| Config file | None |
| Quick run command | Open `https://search.google.com/test/rich-results` and paste URL or code |
| Full suite command | Run all three schema blocks through `https://validator.schema.org/` |

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| SCHEMA-01 | Organization+LocalBusiness JSON-LD validates with zero errors | manual | Paste block into https://validator.schema.org/ | N/A — external tool |
| SCHEMA-01 | Rich Results Test confirms valid markup | manual | https://search.google.com/test/rich-results | N/A — external tool |
| SCHEMA-02 | FAQPage JSON-LD validates with zero errors; covers all 7 Q&A pairs | manual | Paste block into https://validator.schema.org/ | N/A — external tool |
| SCHEMA-02 | Rich Results Test confirms valid FAQPage markup | manual | https://search.google.com/test/rich-results | N/A — external tool |
| SCHEMA-03 | Service JSON-LD validates with zero errors; covers 3 services | manual | Paste block into https://validator.schema.org/ | N/A — external tool |
| SCHEMA-03 | Rich Results Test confirms valid Service markup | manual | https://search.google.com/test/rich-results | N/A — external tool |

**Note:** This is a static HTML project with no test runner. All validation is performed via the two Google/schema.org web tools above. The success criteria is "zero validation errors" — not a SERP appearance (which requires Google crawling, not local testing).

### Sampling Rate

- **Per block written:** Run schema.org validator (paste JSON-LD content)
- **After all three blocks committed:** Run Google Rich Results Test against the deployed URL
- **Phase gate:** All three blocks pass schema.org validator with zero errors before marking phase complete

### Wave 0 Gaps

None — no test infrastructure setup required. External web validators handle all validation. No test files to create.

---

## Environment Availability

| Dependency | Required By | Available | Version | Fallback |
|------------|------------|-----------|---------|----------|
| index.html edit access | All schema blocks | ✓ | — | — |
| schema.org Validator | SCHEMA-01, -02, -03 validation | ✓ (web tool) | — | Google Rich Results Test |
| Google Rich Results Test | Success criteria verification | ✓ (web tool) | — | schema.org Validator |
| Deployed site at leadzly.co | Rich Results Test by URL | ✓ (existing deployment) | — | Paste code snippet directly |

**Missing dependencies with no fallback:** None.

---

## Open Questions

1. **Contact information for SCHEMA-01 contactPoint**
   - What we know: No phone number or email is displayed in the visible page UI. The Calendly booking link is `https://calendly.com/allan-chan-leadzly`.
   - What's unclear: Whether any email (e.g., a contact address) should be added to the visible page before or as part of this phase, so it can be legitimately included in schema.
   - Recommendation: Omit `telephone` and `email` from contactPoint. Use `contactType: "sales"` with `url` pointing to the Calendly booking link as the contact method. Planner should confirm this approach is acceptable.

2. **Street address for LocalBusiness**
   - What we know: The about section says "headquartered in Northern Ireland" — no street address or postcode is given on the page.
   - What's unclear: Whether a registered address exists and should be added to the visible page.
   - Recommendation: Use `addressRegion: "Northern Ireland"` and `addressCountry: "GB"` only. Do not fabricate a street address.

3. **`areaServed` scope**
   - What we know: Hero eyebrow says "UK & Ireland". About section says "Northern Ireland". Meta description says "B2B sales outsourcing."
   - What's unclear: Whether "Ireland" means Republic of Ireland (IE) or just the island.
   - Recommendation: Use `"United Kingdom"` as primary areaServed, with `"Northern Ireland"` as secondary. Omit Republic of Ireland unless the page explicitly claims to serve IE clients.

---

## Sources

### Primary (HIGH confidence)
- Google Developers — Organization schema: https://developers.google.com/search/docs/appearance/structured-data/organization
- Google Developers — FAQPage schema: https://developers.google.com/search/docs/appearance/structured-data/faqpage
- Google Developers — LocalBusiness schema: https://developers.google.com/search/docs/appearance/structured-data/local-business
- schema.org/Service type: https://schema.org/Service
- SEO-Resources skill — schema-markup/SKILL.md (project skill, fetched from GitHub)

### Secondary (MEDIUM confidence)
- index.html FAQ section (lines 1006–1073): all 7 Q&A pairs extracted verbatim
- index.html services section (lines 309–329): all three service names and descriptions
- index.html about section (line 348): business description and parent org

### Tertiary (LOW confidence)
- None

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — JSON-LD is Google's stated preferred format, documented at developers.google.com
- Architecture: HIGH — Schema property requirements verified against official Google and schema.org documentation
- Extracted content: HIGH — Copied verbatim from index.html, no inference
- Pitfalls: HIGH — Verified against official documentation; HTML entity pitfall is a known implementation error

**Research date:** 2026-04-28
**Valid until:** 2026-10-28 (schema.org vocabulary is stable; Google feature eligibility may change sooner)
