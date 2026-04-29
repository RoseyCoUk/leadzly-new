# Phase 12: AEO Copy Optimisation — Research

**Researched:** 2026-04-29
**Domain:** Copy rewriting for Answer Engine Optimisation (AEO) / AI Search extraction
**Confidence:** HIGH (based on direct HTML audit + established AEO copy patterns)

---

## Summary

Phase 12 is a pure copy-editing phase targeting three areas of index.html: the 7 FAQ answers, the hero sub-headline, and the About section paragraph. No structural HTML changes, no CSS changes, no new script blocks are needed.

The core AEO principle is that AI systems (ChatGPT, Perplexity, Google AI Overviews) extract answers that are self-contained in their first sentence. Currently, all 7 FAQ answers open with context-dependent prose — the question's subject must be inferred. Rewriting each to open with a standalone direct-answer sentence is the highest-impact fix.

For hero and About, the problem is vague agency language ("experienced team", "our growth is your growth"). AI systems prefer extractable facts: specific numbers, verifiable claims, named entities. Several statistics already exist on the page (100+ meetings/month, 20+ meetings/month, 12 sectors, 100+ calls/day, 94% show-up rate, £1,500/mo starting price, 30-day kickoff) and must be imported into hero/About copy where they are naturally citeable.

**Primary recommendation:** Edit the 15 text strings across 3 sections in index.html only. No CSS, no JS, no new files. Bump page.css to ?v=15 only if CSS is touched (it should not be).

---

## Current State Audit (extracted from index.html)

### FAQ Section — Current Text

All 7 accordion items at lines 919–988. The visible answer text inside `.faq__answer > p`:

| # | Question | Current Answer (verbatim) |
|---|----------|--------------------------|
| 1 | How quickly can you start generating meetings? | "We typically onboard new clients within 30 days. Week one is ICP audit and script development; week two is outreach infrastructure setup; week three is live calling. Most clients see their first qualified meetings in weeks 3–4." |
| 2 | What does "qualified meeting" actually mean? | "A qualified meeting is a booked appointment with a decision-maker who fits your ICP, understands what you do, and has agreed to the call. We don't count no-shows or rescheduled meetings that never happen." |
| 3 | Do I have to sign a long-term contract? | "No. All plans are month-to-month. We don't believe in locking clients in — if we're not delivering, you should be free to leave. Our 30-day performance guarantee reflects that confidence." |
| 4 | What industries do you work with? | "We've run campaigns across SaaS, recruitment, e-commerce, financial services, professional services, construction, managed IT, and more. If your product or service is sold B2B, we can build a campaign for it." |
| 5 | How is Leadzly different from a typical cold-calling agency? | "Most agencies operate offshore and optimise for call volume. We're a UK-based team that optimises for meeting quality — every lead is qualified before it hits your calendar. You pay for results, not activity." |
| 6 | What information do you need to get started? | "We need your ICP (ideal customer profile), your core value proposition, and access to your calendar or CRM. We handle the rest — list building, scripting, outreach, and qualification." |
| 7 | What happens if you don't hit the meeting targets? | "Our 30-day guarantee means if we don't deliver qualified meetings in your first month, you don't pay for that month. We stand behind our results." |

**AEO diagnosis per answer:**

- Q1: Opens with "We typically onboard" — context-dependent ("we" requires the question). No direct time figure at sentence start.
- Q2: Opens with "A qualified meeting is" — this is actually close to AEO-compliant. Definitional opening is acceptable. Only minor improvement possible.
- Q3: Opens with "No." — good direct answer. The sentence that follows ("All plans are month-to-month.") is self-contained. This answer is already AEO-compliant. Minimal change needed.
- Q4: Opens with "We've run campaigns across" — context-dependent ("we" requires the question). Sector list buried mid-sentence.
- Q5: Opens with "Most agencies operate offshore" — leads with competitor characterisation, not with Leadzly's answer. The actual answer (Leadzly is UK-based, quality-focused) is the second sentence.
- Q6: Opens with "We need your ICP" — context-dependent. "We" and "your" require knowing the question is about getting started.
- Q7: Opens with "Our 30-day guarantee means" — not self-contained; reader needs the question to understand what the guarantee covers.

The JSON-LD FAQPage schema in `<head>` (lines 156–218) mirrors this text **verbatim** and must be updated in sync with visible HTML. Both the visible accordion and the schema must match.

### Hero Sub-Headline — Current Text

Line 351:
```
We run your outbound sales function — telesales, cold email, and LinkedIn — booking qualified meetings directly into your closers' calendars.
```

**AEO diagnosis:** Good structural sentence but contains no verifiable statistics. No number of meetings per month, no sector count, no time-to-start figure. AI systems cannot cite a claim from this sentence — it is all assertion, no data.

### About Section — Current Text

Lines 537–538:
```
Leadzly is a division of Elevateo Co, headquartered in Northern Ireland. We've built our agency on one principle: your growth is our growth. Our team of experienced sales professionals, BDMs, and strategists work as an extension of your business — not just another vendor.
```

**AEO diagnosis:** Contains zero verifiable claims. "Experienced" is vague. "Your growth is our growth" is a brand slogan, not an extractable fact. The only citeable element is "Northern Ireland" and "division of Elevateo Co" — both useful but insufficient alone.

Founder byline (line 538, added in Phase 11):
```
Founded by Allan Chan — sales leader with a track record in NI B2B outbound.
```

"Track record" is vague. No numbers, no verifiable claim.

### page.css version

Line 51: `<link rel="stylesheet" href="./page.css?v=14">`

Current version is **?v=14**. If any CSS is touched in Phase 12 it must be bumped to **?v=15**. Phase 12 is copy-only — no CSS change anticipated, so version should remain at 14.

---

## AEO Requirements Breakdown

### AEO-01 — FAQ Direct Answer Restructure

> All 7 FAQ answers restructured to lead with a direct, self-contained answer sentence before elaborating — optimised for AI extraction and featured snippet eligibility.

**What this means in practice:**
- Every answer's first sentence must be answerable without reading the question
- The first sentence should contain the specific noun (Leadzly, the contract type, the turnaround time) not a pronoun
- The first sentence should be 15–25 words max for featured snippet extraction
- The elaboration can follow as normal prose

### AEO-02 — Hero Statistics

> Hero sub-headline and/or services section copy updated to include at least 2 specific, verifiable statistics.

**Existing statistics already present on the page that can be imported:**
- 100+ meetings/month (trust bar badge, line 426; proven section subtitle line 729)
- 20+ qualified meetings/month (service card, pricing card Growth tier)
- 12 sectors (industries section subtitle: "We've run outbound campaigns across 12 sectors")
- 100+ calls per day (How It Works Step 03)
- 94% show-up rate (hero card stat, decorative)
- From £1,500/mo (comparison table, pricing section)
- 30-day kickoff (hero proof bar)

These are already on the page. The task is to surface 2 of them into the hero sub-headline where they are immediately visible and AI-extractable.

### AEO-03 — About Section Specific Claims

> About section paragraph rewritten with specific, citable claims — NI team, years of experience, or portfolio outcomes — replacing vague agency language.

**Claims available from the page:**
- Division of Elevateo Co (already stated)
- Headquartered in Northern Ireland / Belfast (already in schema, Phase 11 added address)
- 12 sectors covered (industries section)
- 100+ meetings booked monthly (trust bar, proven section)
- Founder: Allan Chan with LinkedIn URL (Phase 11)

**What is NOT available (do not fabricate):**
- Specific years of trading / founding year — not stated anywhere
- Client count — not stated
- Revenue figures — not stated
- Win rates beyond the hero card's decorative 94%

---

## Architecture Patterns

This phase is HTML text-only. No new files, no structural changes.

### Files to Edit

| File | What Changes |
|------|-------------|
| `index.html` | FAQ answer text (lines 925, 935, 944–945, 955, 965, 975, 985), hero sub-headline (line 351), About paragraph (lines 537–538), JSON-LD FAQPage schema (lines 163–218) |

**No other files change.** page.css stays at ?v=14 (no CSS edits). No new script blocks.

### Edit Pattern

For each FAQ answer, the edit is contained inside a `<p>` tag within `.faq__answer`. The matching JSON-LD `text` field must be updated in parallel — the schema text is already plain Unicode (no HTML entities), consistent with the established pattern from Phase 8.

For hero and About, edits are inside `<p>` tags. No class or structure changes.

---

## Recommended Copy Rewrites

### FAQ Rewrites (AEO-01)

**Q1: How quickly can you start generating meetings?**

Current:
> We typically onboard new clients within 30 days. Week one is ICP audit and script development; week two is outreach infrastructure setup; week three is live calling. Most clients see their first qualified meetings in weeks 3–4.

Recommended:
> Leadzly can start generating qualified meetings within 30 days of onboarding. Week one covers ICP audit and script development; week two sets up outreach infrastructure; week three begins live calling. Most clients see their first meetings in weeks 3–4.

*Change: "We typically onboard new clients within 30 days" → "Leadzly can start generating qualified meetings within 30 days of onboarding." — replaces implicit pronoun with named entity, puts the answer (30 days, qualified meetings) at sentence start.*

---

**Q2: What does "qualified meeting" actually mean?**

Current:
> A qualified meeting is a booked appointment with a decision-maker who fits your ICP, understands what you do, and has agreed to the call. We don't count no-shows or rescheduled meetings that never happen.

Recommended (minimal change — already near-compliant):
> A qualified meeting is a confirmed appointment with a decision-maker who fits your ICP, has been briefed on your offering, and has committed to attending. Leadzly does not count no-shows or meetings that reschedule and never take place.

*Change: Replaces informal "understands what you do" with "has been briefed on your offering" (more precise), and "We don't count" with "Leadzly does not count" (removes implicit pronoun in second sentence). Overall structure was already good.*

---

**Q3: Do I have to sign a long-term contract?**

Current:
> No. All plans are month-to-month. We don't believe in locking clients in — if we're not delivering, you should be free to leave. Our 30-day performance guarantee reflects that confidence.

Recommended (minimal change — already AEO-compliant):
> No — Leadzly's plans are all month-to-month with no long-term commitment required. There is no lock-in: if results aren't delivered, clients are free to leave. The 30-day performance guarantee is the clearest demonstration of that commitment.

*Change: "No. All plans are month-to-month." → "No — Leadzly's plans are all month-to-month with no long-term commitment required." — merges the two short sentences, inserts named entity, makes single sentence fully self-contained. Removes "We/we're/Our" implicit pronouns.*

---

**Q4: What industries do you work with?**

Current:
> We've run campaigns across SaaS, recruitment, e-commerce, financial services, professional services, construction, managed IT, and more. If your product or service is sold B2B, we can build a campaign for it.

Recommended:
> Leadzly has run outbound campaigns across 12 B2B sectors, including SaaS, recruitment, e-commerce, financial services, professional services, construction, and managed IT. Any product or service sold B2B is a viable fit for a Leadzly campaign.

*Change: Replaces implicit "We've" with "Leadzly has", adds "12 B2B sectors" (matches the industries section subtitle — existing on-page claim), removes second-sentence pronouns.*

---

**Q5: How is Leadzly different from a typical cold-calling agency?**

Current:
> Most agencies operate offshore and optimise for call volume. We're a UK-based team that optimises for meeting quality — every lead is qualified before it hits your calendar. You pay for results, not activity.

Recommended:
> Leadzly is a UK-based (Northern Ireland) outbound agency that optimises for meeting quality, not call volume. Unlike most agencies that operate offshore, every lead Leadzly books is qualified before it appears in the client's calendar. Clients pay for qualified meetings delivered, not activity metrics.

*Change: Moves Leadzly's differentiator to the first sentence, adds geographic precision (Northern Ireland), removes pronouns throughout.*

---

**Q6: What information do you need to get started?**

Current:
> We need your ICP (ideal customer profile), your core value proposition, and access to your calendar or CRM. We handle the rest — list building, scripting, outreach, and qualification.

Recommended:
> To get started, Leadzly requires three things: your ideal customer profile (ICP), your core value proposition, and access to your calendar or CRM. Leadzly handles everything else — list building, scripting, outreach, and lead qualification.

*Change: Rewrites to open with "To get started, Leadzly requires" (self-contained without question), replaces both "We" instances with "Leadzly".*

---

**Q7: What happens if you don't hit the meeting targets?**

Current:
> Our 30-day guarantee means if we don't deliver qualified meetings in your first month, you don't pay for that month. We stand behind our results.

Recommended:
> If Leadzly does not deliver qualified meetings in a client's first month, that month is not charged — this is the 30-day performance guarantee. Leadzly stands behind its results with a no-quibble refund policy.

*Change: Rewrites to open with "If Leadzly does not deliver" (subject + condition clear without context), removes all "Our/we/our" implicit pronouns, names the guarantee explicitly in the self-contained first sentence.*

---

### Hero Sub-Headline Rewrite (AEO-02)

Current (line 351):
> We run your outbound sales function — telesales, cold email, and LinkedIn — booking qualified meetings directly into your closers' calendars.

Recommended:
> Leadzly runs your outbound sales function — telesales, cold email, and LinkedIn — booking 20+ qualified meetings per month directly into your closers' calendars. No in-house headcount required.

*Change: Adds "20+ qualified meetings per month" (existing on-page stat from pricing/services), adds "No in-house headcount required" (reinforces the core value proposition vs. hiring). The sentence now contains a citeable specific claim.*

Alternative if the team prefers two stats:
> Leadzly runs your full outbound function — telesales, cold email, and LinkedIn — delivering 20+ qualified meetings per month across 12 B2B sectors. Your closers close; we fill their calendars.

*Uses both 20+ meetings and 12 sectors stats. More keyword-dense. Both are existing on-page claims.*

---

### About Section Rewrite (AEO-03)

Current paragraph (lines 537):
> Leadzly is a division of Elevateo Co, headquartered in Northern Ireland. We've built our agency on one principle: your growth is our growth. Our team of experienced sales professionals, BDMs, and strategists work as an extension of your business — not just another vendor.

Current founder byline (line 538):
> Founded by Allan Chan — sales leader with a track record in NI B2B outbound.

Recommended paragraph:
> Leadzly is a B2B sales outsourcing agency headquartered in Belfast, Northern Ireland, and a division of Elevateo Co. The team books over 100 qualified meetings per month across the Elevateo client portfolio, covering 12 sectors including SaaS, recruitment, financial services, and managed IT. Every campaign is managed by a dedicated UK-based sales professional — no offshore teams, no hand-offs to junior callers.

Recommended founder byline:
> Founded by Allan Chan, a Northern Ireland B2B sales leader with hands-on outbound experience across SaaS and professional services.

*Changes: Replaces all vague language with citeable claims. "100 meetings/month" and "12 sectors" are both already stated elsewhere on the page. "Belfast, Northern Ireland" matches the schema address added in Phase 10/11. Founder byline adds sector specificity without fabricating unverifiable claims.*

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead |
|---------|-------------|-------------|
| Schema sync | A script to auto-sync FAQ HTML and JSON-LD | Manual edit of both at same time — the schema is small, 7 Q&As |
| AEO scoring | A custom readability scorer | Visual review: does the first sentence answer the question without the question? |

---

## Common Pitfalls

### Pitfall 1: Updating HTML but not JSON-LD
**What goes wrong:** The visible FAQ text is updated, but the FAQPage JSON-LD in `<head>` is not. Google and AI systems parse both and may prefer the schema. Inconsistency can suppress the featured snippet.
**How to avoid:** Edit JSON-LD at lines 163–218 in the same commit as the visible HTML.
**Warning signs:** If the plan has separate tasks for HTML and schema, merge them.

### Pitfall 2: Using HTML entities in JSON-LD text values
**What goes wrong:** Writing `&ndash;` or `&rsquo;` inside the JSON `text` field. JSON does not decode HTML entities.
**How to avoid:** Use plain Unicode — `–` (en-dash), `'` (right single quote), `"` (left double), `"` (right double). This is already the established pattern from Phase 8 (KEY DECISION in STATE.md).
**Warning signs:** Any `&` character inside a JSON string.

### Pitfall 3: Fabricating statistics
**What goes wrong:** Inserting a specific number that does not already appear on the page (e.g., "5 years of experience", "200+ clients").
**How to avoid:** Only use statistics that already appear on the page: 20+, 100+, 12, 94%, 30 days, £1,500.
**Warning signs:** Any stat in copy that cannot be found elsewhere in index.html.

### Pitfall 4: Breaking the hero card's decorative stats
**What goes wrong:** Editing the hero section's `<p class="hero__sub">` but accidentally touching the hero card HTML or the counter JS.
**How to avoid:** The hero sub-headline is a single `<p>` tag at line 351. The hero card is a separate `<div class="hero__card">` at line 366. They are siblings, not nested.

### Pitfall 5: Page CSS version bump when no CSS changed
**What goes wrong:** Bumping page.css from ?v=14 to ?v=15 in the `<link>` tag when no CSS changes were made, causing unnecessary cache invalidation.
**How to avoid:** Only bump if a CSS file was actually changed. Phase 12 is copy-only — no bump expected.

---

## Implementation Approach

### What to Edit

**File: index.html only**

1. **Hero sub-headline** — line 351, inside `<p class="hero__sub">`. Replace the single `<p>` text content.

2. **About section paragraph** — lines 537–538, inside the `<p class="about__text">` and `<p class="about__founder">`. Replace both paragraph text contents.

3. **FAQ visible answers** — 7 `<p>` tags inside `.faq__answer` divs (lines 925, 935, 944, 955, 965, 975, 985 approximately). Note that HTML entities like `&rsquo;`, `&mdash;`, `&ndash;` are appropriate inside HTML `<p>` tags (unlike JSON-LD).

4. **FAQ JSON-LD schema** — lines 163–218, the `FAQPage` `acceptedAnswer.text` values for all 7 questions. These must use plain Unicode (no HTML entities). Update to match new visible answer text.

### Plan Structure Recommendation

Two plans:
- **Plan 01**: FAQ answer restructure — both visible HTML and JSON-LD FAQPage schema (AEO-01)
- **Plan 02**: Hero and About copy update (AEO-02 + AEO-03)

This separates concerns: Plan 01 is the longer editing task (7 answers × 2 locations), Plan 02 is the shorter targeted copy update.

---

## Validation Architecture

nyquist_validation is enabled in config.json.

### Test Framework

| Property | Value |
|----------|-------|
| Framework | Manual browser review — no automated test framework on this static HTML project |
| Config file | None |
| Quick run command | Open index.html in browser, expand each FAQ accordion, visually verify |
| Full suite command | Same as above + view-source to verify JSON-LD text values |

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| AEO-01 | Each FAQ answer first sentence is self-contained without reading the question | manual | Open index.html in browser, read first sentence of each expanded FAQ item | N/A — HTML review |
| AEO-01 | JSON-LD FAQPage text values match visible HTML answers | manual | View-source → search for `FAQPage` → compare `text` values with visible HTML | N/A — source review |
| AEO-02 | Hero sub-headline contains at least 2 specific verifiable statistics | manual | View hero section, count stats with numerical values | N/A — content review |
| AEO-03 | About paragraph contains no vague language; all claims are specific and verifiable from page content | manual | Read About section, flag any unverified claims | N/A — content review |

### Sampling Rate

- **Per task commit:** Visual review of changed section in browser
- **Per plan completion:** Full page scroll-through, verify all modified sections
- **Phase gate:** All 7 FAQ answers pass self-containment test, hero has 2+ stats, About has 0 vague unsubstantiated phrases

### Wave 0 Gaps

None — existing infrastructure covers all phase requirements. This is pure copy editing on an existing static HTML file. No test files need to be created.

---

## Sources

### Primary (HIGH confidence)
- Direct HTML audit of `index.html` — all current text extracted verbatim from lines 351, 537–538, 919–988, 163–218
- `.planning/REQUIREMENTS.md` — AEO-01, AEO-02, AEO-03 requirements read directly
- `.planning/STATE.md` — confirmed page.css is at ?v=14, JSON-LD pattern (no HTML entities in JSON), established per-session decisions

### Secondary (MEDIUM confidence)
- AEO copy principles (direct answer first, named entity not pronoun, specific stats) — well-established SEO/AEO best practice, consistent with featured snippet optimisation guidelines
- The "Leadzly" named entity substitution for "We" pronoun in AEO context — standard AI citation pattern; AI systems attribute quoted text to named entities, not pronouns

### Tertiary (LOW confidence)
- Specific AI model extraction behaviour (GPT-4, Perplexity, Gemini) — not formally tested against this specific content; general pattern well-documented but exact threshold for extraction not verifiable

---

## Environment Availability

Step 2.6: SKIPPED — Phase 12 is pure copy/text editing of a static HTML file. No external tools, services, CLIs, or runtimes are required beyond a text editor and browser.

---

## State of the Art

| Old Approach | Current Approach | Impact |
|--------------|-----------------|--------|
| FAQ answers written for human readers (context assumed) | FAQ answers written for AI extraction (self-contained, entity-named) | AI systems can extract and cite individual answers independently |
| Vague agency copy ("experienced team") | Specific verifiable claims (sector count, meeting volume) | AI systems can quote Leadzly as a source with supporting data |

---

## Open Questions

1. **Founder experience specificity**
   - What we know: Allan Chan is a NI B2B sales leader with LinkedIn profile
   - What's unclear: Whether "years of experience" or a specific founding year is available/accurate
   - Recommendation: Do not invent a year or duration. Use sector-specific description only (SaaS, professional services) as already recommended above.

2. **Hero sub-headline: one stat or two**
   - What we know: "20+ meetings/month" is the clearest stat; "12 sectors" is secondary
   - What's unclear: Whether cramming two stats into one sentence hurts readability/conversion
   - Recommendation: Use the primary rewrite (20+ meetings/month only) as the conservative choice. Flag the two-stat alternative in the plan for human decision.

3. **JSON-LD sync scope**
   - What we know: FAQPage schema text values must match visible HTML exactly
   - What's unclear: Whether minor wording differences between schema and HTML trigger a penalty vs. tolerance
   - Recommendation: Make schema text identical to visible HTML (after stripping HTML entities to Unicode). Exact match is safest.

---

## Metadata

**Confidence breakdown:**
- Current state audit: HIGH — read directly from index.html
- Recommended rewrites: MEDIUM-HIGH — based on established AEO/featured-snippet copy principles; specific wording is editorial judgment
- Implementation approach: HIGH — file/line locations confirmed from direct HTML read
- Validation: HIGH — project has no automated test framework, manual review is the established pattern

**Research date:** 2026-04-29
**Valid until:** 2026-05-29 (stable domain — copy changes, no dependency on external libraries)
