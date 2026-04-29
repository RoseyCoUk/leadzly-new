# Phase 10: Schema Authority Layer — Research

**Researched:** 2026-04-28
**Domain:** JSON-LD structured data — LocalBusiness update, Person entity, HowTo block
**Confidence:** HIGH

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| NAP-03 | Existing `LocalBusiness` JSON-LD updated with `streetAddress`, `addressLocality` (Belfast), `postalCode` (BT5 5JE), `addressCountry` (GB), and `telephone` | PostalAddress properties and telephone confirmed valid by Google LocalBusiness docs; exact values sourced from STATE.md |
| EEAT-02 | `Person` JSON-LD block added to `<head>` for Allan Chan (name, jobTitle "Founder", url `https://leadzly.co`, sameAs LinkedIn URL) | All four properties confirmed valid on schema.org/Person; LinkedIn URL sourced from STATE.md |
| SCHEMA-04 | `HowTo` JSON-LD block added to `<head>` mapping the 3-step "How It Works" process (step names, descriptions, and URLs matching visible section content) | HowToStep name/text/url properties confirmed valid; step content sourced verbatim from index.html |

</phase_requirements>

---

## Project Constraints (from CLAUDE.md)

- **Tech stack:** HTML/CSS/vanilla JS only — no build system, no npm, no frameworks
- **JSON-LD blocks:** Must go in `<head>` before closing `</head>` tag (established pattern from Phase 8)
- **No HTML entities in JSON:** All string values must use plain Unicode, not HTML entity codes
- **Separate blocks:** One `<script type="application/ld+json">` per schema type (pattern established in Phase 8)
- **Content match:** Every value in JSON-LD must correspond to content visible on the page or in validated business data (STATE.md)

---

## Summary

Phase 10 adds three JSON-LD edits/blocks to the `<head>` of index.html:

1. **Update** the existing LocalBusiness block (SCHEMA-01) with full PostalAddress fields (streetAddress, addressLocality, postalCode, addressCountry) and the telephone number.
2. **Add** a new Person block for Allan Chan with jobTitle, url, and sameAs (LinkedIn).
3. **Add** a new HowTo block mapping the visible "How It Works" section.

This is a pure HTML edit — no CSS, no JS, no new files. All three tasks follow patterns already established in Phase 8. The only new schema types are Person and HowTo; both are well-documented on schema.org.

**Critical detail on step count:** The visible "How It Works" section in index.html contains 4 steps (ICP Audit, Custom Script, Daily Outreach, Qualified Handoff). The phase success criteria state "3 steps." The planner must resolve this conflict. The safest approach is to include all 4 visible steps in the HowTo block — schema validators do not penalise additional steps, and the success criterion "names and descriptions match the visible section" is best satisfied by including all visible steps. If the requirement intends only 3 steps, the planner should surface this to the user.

**Primary recommendation:** Update the LocalBusiness block in place (edit the existing `<script>` block), then append two new `<script type="application/ld+json">` blocks (Person, HowTo) immediately after the existing schema blocks in `<head>`.

---

## Standard Stack

### Core

| Library / Tool | Version | Purpose | Why Standard |
|----------------|---------|---------|--------------|
| JSON-LD | — | Machine-readable schema embedded in HTML | Google's preferred structured data format; no external dependency |
| schema.org vocabulary | — | Defines type and property names | Universal; validated by Google and schema.org validator |

### Supporting

| Tool | Purpose | When to Use |
|------|---------|-------------|
| schema.org Validator (https://validator.schema.org/) | Zero-error validation before commit | Paste each JSON-LD block to confirm no parse or property errors |
| Google Rich Results Test (https://search.google.com/test/rich-results) | Confirm schema eligibility | After deploying; confirms LocalBusiness and HowTo rich result eligibility |

### Alternatives Considered

| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Editing existing SCHEMA-01 block in place | Adding a second LocalBusiness block | Two conflicting LocalBusiness blocks cause validator warnings; edit in place is correct |
| Separate `<script>` blocks | Single `@graph` combining all types | Separate blocks match Phase 8 convention, easier to validate and debug individually |

**Installation:** None. JSON-LD is a `<script>` tag with no external dependency.

---

## Architecture Patterns

### Recommended Structure

Three changes to `<head>` in this order:

1. **Edit** existing SCHEMA-01 `<script>` block — add `streetAddress`, `addressLocality`, `postalCode` to the `address` object; add `telephone` at top level
2. **Append** new Person block after the existing three schema blocks
3. **Append** new HowTo block after the Person block

### Pattern 1: LocalBusiness Address Update (NAP-03)

The existing `address` object in SCHEMA-01 currently contains only `addressRegion` and `addressCountry`. Replace it with the full PostalAddress:

```json
"address": {
  "@type": "PostalAddress",
  "streetAddress": "1 Hollycroft Avenue",
  "addressLocality": "Belfast",
  "addressRegion": "Northern Ireland",
  "postalCode": "BT5 5JE",
  "addressCountry": "GB"
},
"telephone": "+13074009814"
```

**Source for values:** STATE.md v1.2 note — "NAP address: 1 Hollycroft Avenue, Belfast, BT5 5JE, United Kingdom | Phone: +1 307 400 9814"

**Telephone formatting:** Google recommends E.164 format (no spaces or dashes): `+13074009814`. The display-friendly form `+1 307 400 9814` is also accepted but E.164 is preferred for machine parsing.

**Schema-content note from Phase 8:** Phase 8 deliberately omitted street address and telephone because they were not visible on the page. Phase 11 will add them as visible elements. The planner must decide whether to add the schema first (Phase 10) before the visible content (Phase 11), or defer NAP-03 until Phase 11 adds visible NAP. The ROADMAP.md explicitly places NAP-03 in Phase 10, so proceed — Google accepts schema data that the business can verify, even if not yet surfaced as page text. The Google Rich Results Test verifies schema validity, not page-content parity for LocalBusiness (unlike FAQPage).

### Pattern 2: Person Entity (EEAT-02)

New block, placed after the existing three schema blocks:

```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Allan Chan",
  "jobTitle": "Founder",
  "url": "https://leadzly.co",
  "sameAs": "https://www.linkedin.com/in/allan-chan-865422216/"
}
```

**All four properties confirmed valid** on schema.org/Person:
- `name` — inherited from Thing
- `jobTitle` — native Person property
- `url` — inherited from Thing
- `sameAs` — inherited from Thing; standard pattern for linking to social/professional profiles

**sameAs as string vs array:** When there is only one external identity URL, a single string value is valid. An array is also valid: `"sameAs": ["https://www.linkedin.com/in/allan-chan-865422216/"]`. Either form passes the validator.

### Pattern 3: HowTo Block (SCHEMA-04)

New block, placed after the Person block. Uses HowToStep with `name`, `text`, and `url` properties:

```json
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "From Zero to Pipeline in 4 Steps",
  "description": "How Leadzly books qualified B2B meetings for clients through a four-step outreach process.",
  "step": [
    {
      "@type": "HowToStep",
      "position": "1",
      "name": "ICP Audit",
      "text": "We analyse your ideal customer profile, market positioning, and competitive landscape.",
      "url": "https://leadzly.co/#how-it-works"
    },
    {
      "@type": "HowToStep",
      "position": "2",
      "name": "Custom Script",
      "text": "Our team builds tailored call scripts, email sequences, and objection-handling frameworks.",
      "url": "https://leadzly.co/#how-it-works"
    },
    {
      "@type": "HowToStep",
      "position": "3",
      "name": "Daily Outreach",
      "text": "Dedicated reps make 100+ calls per day, targeting qualified decision-makers.",
      "url": "https://leadzly.co/#how-it-works"
    },
    {
      "@type": "HowToStep",
      "position": "4",
      "name": "Qualified Handoff",
      "text": "Warm, qualified meetings land directly on your calendar. You just close.",
      "url": "https://leadzly.co/#how-it-works"
    }
  ]
}
```

**Property notes:**
- `name` — required on HowTo; use the visible section heading "From Zero to Pipeline in 4 Steps"
- `description` — recommended; write a plain-English sentence summarising the process
- `step` — array of HowToStep objects (not `steps` — the property is `step` on HowTo)
- `position` — string value indicating ordering; helps parsers confirm sequence
- `text` — the step description text (equivalent role to `description` on HowToStep, but `text` is the correct property name inherited from CreativeWork)
- `url` — inherited from Thing; valid on HowToStep; use the page anchor `https://leadzly.co/#how-it-works` for all steps since the section has a single anchor, not per-step anchors

**Step count discrepancy:** The success criterion says "3 steps" but the visible section has 4. Research recommendation: include all 4 steps to match the visible content. The criterion "names and descriptions match the visible section content" is better satisfied by including all steps. Flag this in the plan for the planner to confirm.

### Anti-Patterns to Avoid

- **Duplicate LocalBusiness block:** Do not add a second LocalBusiness block. Edit the existing SCHEMA-01 block in place.
- **HTML entities in JSON strings:** All step descriptions are copied from HTML. Verify no HTML entities (`&amp;`, `&ndash;`, `&rsquo;`) remain in the JSON values.
- **Using `steps` instead of `step`:** The HowTo property name is `step` (singular), not `steps`. Using `steps` is a string property (plain text description), not the array of HowToStep objects.
- **Omitting `@type: "HowToStep"` on each step:** Without it, parsers cannot identify the step objects.
- **Wrong telephone format:** Do not use `"telephone": "+1 307 400 9814"` with spaces — use `"+13074009814"` (E.164) or the spaced form consistently. Either passes schema.org validator; E.164 is safer.

---

## Extracted Content (Verbatim from index.html)

### NAP Data (for NAP-03)

| Property | Value | Source |
|----------|-------|--------|
| streetAddress | 1 Hollycroft Avenue | STATE.md v1.2 note |
| addressLocality | Belfast | STATE.md v1.2 note |
| addressRegion | Northern Ireland | Existing SCHEMA-01 (keep) |
| postalCode | BT5 5JE | STATE.md v1.2 note |
| addressCountry | GB | Existing SCHEMA-01 (keep) |
| telephone | +1 307 400 9814 | STATE.md v1.2 note |

### Person Data (for EEAT-02)

| Property | Value | Source |
|----------|-------|--------|
| name | Allan Chan | STATE.md v1.2 note |
| jobTitle | Founder | REQUIREMENTS.md EEAT-02 |
| url | https://leadzly.co | REQUIREMENTS.md EEAT-02 |
| sameAs | https://www.linkedin.com/in/allan-chan-865422216/ | STATE.md v1.2 note |

### How It Works Steps (for SCHEMA-04, verbatim from index.html lines 570–591)

| Step | Name | Text (decoded) |
|------|------|----------------|
| 1 | ICP Audit | We analyse your ideal customer profile, market positioning, and competitive landscape. |
| 2 | Custom Script | Our team builds tailored call scripts, email sequences, and objection-handling frameworks. |
| 3 | Daily Outreach | Dedicated reps make 100+ calls per day, targeting qualified decision-makers. |
| 4 | Qualified Handoff | Warm, qualified meetings land directly on your calendar. You just close. |

**Section heading (for HowTo `name`):** "From Zero to Pipeline in 4 Steps"
**Section anchor:** `id="how-it-works"` on the `<section>` element
**No HTML entities detected** in any of the four step descriptions — safe to use verbatim.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Structured data format | Custom meta tags | JSON-LD script blocks | Google's preferred format; no DOM changes required |
| Validation | Manual inspection | schema.org Validator + Rich Results Test | Catches property-level errors automatically |
| Schema vocabulary | Invented property names | schema.org standard properties | Non-standard properties are ignored by all parsers |

---

## Common Pitfalls

### Pitfall 1: Editing the Wrong Script Block
**What goes wrong:** Multiple `<script type="application/ld+json">` blocks exist. Developer edits SCHEMA-02 (FAQPage) instead of SCHEMA-01 (LocalBusiness) when adding address fields.
**Why it happens:** Blocks look identical at a glance; the `<!-- comment -->` label above each block distinguishes them.
**How to avoid:** Use the HTML comment `<!-- Structured Data: Organization + LocalBusiness (SCHEMA-01) -->` as the anchor for the edit. Verify the block being edited has `"@type": ["LocalBusiness", "ProfessionalService"]`.
**Warning signs:** Validator shows address properties on the wrong schema type.

### Pitfall 2: `steps` vs `step` Property Name
**What goes wrong:** `"steps"` (plural) is a plain-text string property on HowTo for simple instructions. The array of HowToStep objects belongs under `"step"` (singular).
**Why it happens:** Intuitive pluralisation of an array property.
**How to avoid:** Always use `"step"` when attaching HowToStep objects. Validator will report "step" as valid and ignore "steps" if it contains an array.
**Warning signs:** Validator shows HowTo with no steps detected.

### Pitfall 3: Schema-Content Mismatch on Person
**What goes wrong:** Adding Person schema with jobTitle "Founder" before Allan Chan is named as founder anywhere on the page (Phase 11 does that).
**Why it happens:** Schema is correct but content lags behind.
**Note:** Google's validator validates schema syntax, not page-content parity for Person entities. The Rich Results Test for Person does not check that the name appears visibly on the page. This is lower risk than FAQPage/HowTo where content parity is more strictly evaluated.

### Pitfall 4: HTML Entities in JSON Step Text
**What goes wrong:** Copy-paste from HTML source leaves `&mdash;` or `&rsquo;` in JSON string values.
**Why it happens:** The HTML source uses entities; the JSON requires Unicode.
**How to avoid:** The four step descriptions in index.html do not use any HTML entities — they are safe to copy verbatim. Confirm before writing.
**Warning signs:** Validator reports unexpected string characters.

### Pitfall 5: HowTo Rich Result Eligibility
**What goes wrong:** HowTo schema is valid but does not generate rich results in SERPs.
**Why it happens:** Google restricts HowTo rich results. As of 2024–2025, HowTo rich snippets appear primarily for instructional/DIY content; B2B process descriptions are less likely to trigger the feature. However, the schema still signals process structure to AI systems and knowledge graph crawlers.
**How to avoid:** Implement the schema regardless — the success criterion is validator zero-errors, not a SERP feature. Do not skip SCHEMA-04 because of SERP eligibility uncertainty.

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Address with only `addressRegion` | Full PostalAddress with all five sub-fields | Google recommendation, ongoing | Higher-quality knowledge panel data |
| Person schema without `sameAs` | Person with `sameAs` LinkedIn for identity disambiguation | E-E-A-T emphasis 2023–2025 | Connects author entity to verified external identity |
| HowTo with plain text `steps` string | HowTo with `step` array of HowToStep objects | schema.org v3+, ongoing | Enables step-level rich result rendering |

**Deprecated/outdated:**
- `steps` as array: Using `"steps"` (plural) as an array property is not the correct schema.org pattern. Use `"step"` (singular) with an array of HowToStep objects.

---

## Open Questions

1. **Step count: 3 vs 4**
   - What we know: The visible "How It Works" section has 4 steps. The phase success criterion says "3 steps."
   - What's unclear: Whether the criterion intends to describe the current visible content (which has been 4 steps since Phase 2) or whether a future content change to 3 steps is implied.
   - Recommendation: Include all 4 visible steps in the HowTo block. Surface this to the planner as a flag item. The schema should match the visible page, not an arbitrary step count.

2. **Telephone format in schema**
   - What we know: The NAP telephone is +1 307 400 9814. Google accepts both spaced and E.164 formats.
   - Recommendation: Use E.164 format `+13074009814` in the JSON-LD for maximum parser compatibility. The visible footer (Phase 11) can use the human-readable `+1 307 400 9814` form.

3. **NAP schema before NAP visible content**
   - What we know: Phase 10 adds NAP to schema; Phase 11 adds NAP as visible page elements. The order is schema-first.
   - What's unclear: Whether Google may flag LocalBusiness schema address fields that do not yet appear as visible text on the page.
   - Recommendation: Proceed as planned. Google's LocalBusiness validator checks schema syntax and property correctness, not page-content parity (unlike FAQPage). The address data is real business data. No risk of a validation error from this sequencing.

---

## Environment Availability

| Dependency | Required By | Available | Version | Fallback |
|------------|------------|-----------|---------|----------|
| index.html edit access | All three schema changes | ✓ | — | — |
| schema.org Validator | NAP-03, EEAT-02, SCHEMA-04 validation | ✓ (web tool) | — | Google Rich Results Test |
| Google Rich Results Test | Success criteria verification | ✓ (web tool) | — | schema.org Validator |
| Deployed site at leadzly.co | Rich Results Test by URL | ✓ (existing) | — | Paste code snippet directly |

**Missing dependencies with no fallback:** None.

---

## Validation Architecture

### Test Framework

| Property | Value |
|----------|-------|
| Framework | Manual validation via external tools (static HTML project, no test runner) |
| Config file | None |
| Quick run command | Paste JSON-LD block into https://validator.schema.org/ |
| Full suite command | Run all three blocks through https://validator.schema.org/ + https://search.google.com/test/rich-results |

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| NAP-03 | LocalBusiness JSON-LD contains streetAddress, postalCode, addressLocality, addressCountry, telephone with correct values | manual | Paste updated SCHEMA-01 block into https://validator.schema.org/ | N/A — external tool |
| NAP-03 | Rich Results Test confirms LocalBusiness schema valid | manual | https://search.google.com/test/rich-results | N/A — external tool |
| EEAT-02 | Person JSON-LD block present in head with name, jobTitle, url, sameAs fields | manual | Paste Person block into https://validator.schema.org/ | N/A — external tool |
| SCHEMA-04 | HowTo JSON-LD block present in head with step array matching visible section | manual | Paste HowTo block into https://validator.schema.org/ | N/A — external tool |

### Sampling Rate

- **Per block written/edited:** Paste into schema.org validator before committing
- **After all changes committed:** Run Google Rich Results Test against deployed URL
- **Phase gate:** All three blocks (updated LocalBusiness, Person, HowTo) pass schema.org validator with zero errors

### Wave 0 Gaps

None — no test infrastructure setup required. External web validators handle all validation.

---

## Sources

### Primary (HIGH confidence)
- [schema.org/Person](https://schema.org/Person) — name, jobTitle, url, sameAs properties confirmed valid
- [schema.org/HowTo](https://schema.org/HowTo) — step array structure confirmed
- [schema.org/HowToStep](https://schema.org/HowToStep) — name, text, url properties confirmed valid (inherited from Thing/CreativeWork)
- [Google LocalBusiness structured data](https://developers.google.com/search/docs/appearance/structured-data/local-business) — streetAddress, postalCode, addressLocality, addressCountry, telephone confirmed recommended properties
- index.html lines 122–149 — existing SCHEMA-01 LocalBusiness block (edit target)
- index.html lines 570–591 — "How It Works" section, all 4 step names and descriptions verbatim
- STATE.md v1.2 notes — NAP address, telephone, Allan Chan LinkedIn URL

### Secondary (MEDIUM confidence)
- [optimizeup.com Person schema 2025](https://optimizeup.com/schema-org-person-markup-identity-branding-2025/) — sameAs LinkedIn pattern confirmed as current practice
- [HowTo JSON-LD guide (Unhead)](https://unhead.unjs.io/docs/schema-org/api/schema/how-to) — HowToStep structure examples

### Tertiary (LOW confidence)
- None

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — JSON-LD on schema.org, confirmed via official docs
- Architecture: HIGH — all property names verified against schema.org type pages
- Extracted content: HIGH — copied verbatim from index.html and STATE.md, no inference
- Pitfalls: HIGH — based on Phase 8 established patterns + schema.org property verification

**Research date:** 2026-04-28
**Valid until:** 2026-10-28 (schema.org vocabulary is stable; Google SERP feature eligibility may change faster)
