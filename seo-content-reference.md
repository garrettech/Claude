# SEO Content Reference

**Purpose:** A reusable, brand-agnostic reference for Claude to execute SEO and performance content tasks across any brand owned by Kris (Les Néréides Australia, Love Persimmon, Roberto Verino ANZ, Mr. Boho) or future brands. Covers blogs, product pages, collection pages, category/home/landing pages, meta-only rewrites, Google Ads, Meta Ads, and email content optimisation.

**Repository:** `garrettech/claude`
**Companion files:** `[brand name] brand voice.json` (one per brand, same repo)
**Document owner:** Kris
**Last updated:** *(update on each revision)*

---

## 1. How Claude Should Use This Document

1. Read this file end-to-end before beginning any SEO content task.
2. Read the relevant `[brand name] brand voice.json` file for the brand in scope. Brand voice JSON **overrides** any generic voice guidance here.
3. Classify the request into one of the supported content types in Section 4.
4. Confirm mandatory inputs before drafting (see Section 10).
5. Execute the workflow for that content type.
6. Enrich with available data (GSC, GA4, SEMrush, Windsor.ai, Klaviyo, web search — whatever is connected).
7. Review against the quality frameworks in Section 8.
8. Return the final output in the prescribed schema with an overall quality rating out of 10.

If required information is missing, ask for it before drafting. Never invent brand facts, product specs, or statistics.

---

## 2. Brand Voice File — Expected Schema

Each brand voice JSON should contain at minimum:

```json
{
  "brand_name": "",
  "market": "",
  "website_url": "",
  "category": "",
  "positioning": "",
  "tone_descriptors": [],
  "voice_principles": [],
  "vocabulary_favour": [],
  "vocabulary_avoid": [],
  "target_audience": {
    "primary": "",
    "secondary": "",
    "gift_shoppers": ""
  },
  "usp": [],
  "forbidden_claims": [],
  "competitor_policy": "",
  "spelling_convention": ""
}
```

If any field is missing from the brand voice file, ask Kris before drafting. Do not infer brand voice from the website alone — published copy may itself be in need of rewrite.

---

## 3. Global Rules (Apply to Every Content Type)

**Always:**
- Write in original language; never copy from source URLs or competitor sites.
- Use the brand's tone, vocabulary, and spelling convention as defined in the brand voice JSON.
- Prioritise human readability over keyword density.
- Match search intent before optimising for volume.
- Cite only verified brand facts (from brand voice JSON, provided URLs, uploaded assets, or confirmed ecommerce data).
- Use British/Australian English unless the brand voice JSON specifies otherwise.

**Never:**
- Mention competitor brands or products.
- Fabricate materials, dimensions, prices, awards, or statistics.
- Keyword-stuff or use unnatural exact-match repetition.
- Use off-brand vocabulary listed in the brand's "vocabulary_avoid" array.
- Make major structural changes when integrating keyword research results (see Section 7).
- Promise results, rankings, or sales outcomes.

---

## 4. Supported Content Types

| # | Type | Primary goal |
|---|------|---|
| A | Blog article | Top-of-funnel organic traffic, intent capture, brand authority |
| B | Product page | Mid/bottom-funnel conversion, long-tail organic |
| C | Collection / category page | Mid-funnel organic, internal link hub |
| D | Home page | Brand ranking, navigation, trust |
| E | Landing page | Campaign conversion, paid traffic destination |
| F | Meta-only rewrite | Title tag + meta description refresh (no body change) |
| G | Google Ads copy | Responsive Search Ads, assets, sitelinks |
| H | Meta Ads copy | Primary text, headlines, descriptions across placements |
| I | Email content | Subject lines, preview text, body optimisation for engagement |

Email is included under "SEO-adjacent" — it does not affect organic search, but the same intent-matching and engagement principles apply.

---

## 5. Workflows by Content Type

### A. Blog Article

**Mandatory inputs:**
- Primary keyword
- Title: either user-supplied or request Claude to generate
- Secondary keywords (optional but recommended)

**Workflow:**
1. Confirm primary keyword, title preference, secondary keywords.
2. Classify search intent (informational / commercial / transactional / navigational).
3. Produce:
   - SEO title tag (≤60 characters, primary keyword included)
   - Meta description (≤160 characters, primary keyword included)
   - Suggested URL slug (short, keyword-led, hyphenated)
   - Outline with H2/H3 hierarchy
   - Full article: 800–1,200+ words
   - Opening hook + primary keyword in first paragraph
   - Natural keyword placement across H2s, body, and FAQ
   - Bulleted or numbered lists where useful
   - 2–3 suggested internal links (to relevant collections, products, or existing blogs)
   - 1–2 suggested external authoritative links (industry bodies, reputable publications — never competitors)
   - Image placeholders with descriptive alt text (include primary keyword naturally in at least one)
   - Conclusion with clear CTA
   - 5 unique FAQs (each a distinct query, not paraphrases of each other)
   - Bold article title and all headings
4. Request keyword enrichment from GSC, GA4, SEMrush, or any available keyword tool, OR from a user-supplied keyword list.
5. Integrate enrichment keywords naturally — no major structural change.
6. Review against the Helpful Content Guidelines (Section 8).
7. Rate across the 5 Search Quality dimensions (Section 8).
8. Revise to lift any weak dimension.
9. Return final article + overall rating out of 10 + compliance note.

### B. Product Page

**Mandatory inputs:**
- Product page URL
- Focus keyword

**Workflow:**
1. Read the current product page content (via URL or user paste).
2. Preserve all verified product facts (materials, dimensions, price, SKU-level specs).
3. Rewrite:
   - H1 / product name (retain exact product name unless user approves change)
   - Opening SEO paragraph (≥500 characters total rewrite)
   - Top 3 benefits (scannable, outcome-led)
   - Product story / craftsmanship / origin section
   - Styling or gifting angle
   - CTA
   - 3–5 FAQs addressing common pre-purchase questions
4. Include alt text suggestions for primary product imagery.
5. Request keyword enrichment (as per blog workflow).
6. Integrate naturally — no major structural change.
7. Rate on the 5 Search Quality dimensions.
8. Revise.
9. Return final copy + overall rating out of 10.

### C. Collection / Category Page

**Mandatory inputs:**
- Collection page URL
- Focus keyword
- At least 2 internal blog or content URLs to integrate

**Workflow:**
1. Read current collection content.
2. Rewrite:
   - Top intro section (above product grid)
   - Bottom SEO section (below product grid)
   - Minimum 500 characters total across both sections
3. Integrate the 2+ blog links naturally within the body text — not as a list dump.
4. Suggest an optimised SEO title tag and meta description for the collection page.
5. Request keyword enrichment.
6. Integrate naturally.
7. Rate on the 5 Search Quality dimensions.
8. Revise.
9. Return final copy + overall rating out of 10.

### D. Home Page

**Mandatory inputs:**
- Current home page URL or content
- Brand primary keyword (typically the brand name + category, e.g. *"French jewellery Australia"*)

**Workflow:**
1. Rewrite in the following order:
   - Hero headline and subheadline
   - Value proposition block
   - Category/collection highlight blocks with short SEO-bearing intros
   - Brand story short-form
   - Trust block (reviews, press, credentials — only if verified)
   - Footer CTA
2. Suggest SEO title tag and meta description.
3. Suggest internal linking pattern across navigation/footer.
4. Request keyword enrichment.
5. Rate, revise, return with rating out of 10.

### E. Landing Page

**Mandatory inputs:**
- Campaign objective (what action should a visitor take?)
- Traffic source (Google Ads, Meta Ads, email, organic)
- Focus keyword or campaign angle
- Offer / promotion details (if any)

**Workflow:**
1. Match the message to the ad/email that drove the click (message match).
2. Draft:
   - Hero headline (mirrors ad promise)
   - Subheadline (expands benefit)
   - 3–5 benefit bullets
   - Social proof block (only if verified)
   - Primary CTA (above fold)
   - Objection-handling section / FAQ
   - Secondary CTA
3. Keep the page focused on one conversion action. Remove navigation distractions in the recommendation.
4. Suggest SEO title tag and meta description (even for paid pages — useful for direct traffic and brand SERPs).
5. Rate, revise, return with rating out of 10.

### F. Meta-Only Rewrite

**Mandatory inputs:**
- Page URL
- Page type (home / collection / product / blog / other)
- Focus keyword

**Workflow:**
1. Read current title tag and meta description if supplied; otherwise infer from URL and page content.
2. Draft 3 title tag variants (≤60 characters each, primary keyword included).
3. Draft 3 meta description variants (≤160 characters each, primary keyword + benefit + CTA).
4. Recommend the strongest option with a one-line rationale.
5. Flag any existing title/meta issues (truncation risk, keyword missing, weak CTA, duplicate across site).

### G. Google Ads Copy (Responsive Search Ads)

**Mandatory inputs:**
- Target landing page URL
- Focus keyword / keyword theme
- Unique selling points / current offer
- Brand voice JSON

**Workflow:**
1. Produce a full RSA asset set:
   - 15 headlines (≤30 characters each, include primary keyword in at least 3, include brand name in at least 2, include CTA in at least 2, include USPs across remainder)
   - 4 descriptions (≤90 characters each, benefit + CTA)
   - Final URL and display URL path suggestions (2 path fields, ≤15 characters each)
2. Propose callout assets (≤25 characters, 8–10 suggestions).
3. Propose sitelink assets (4–6 suggestions: title ≤25 chars, 2 descriptions ≤35 chars each).
4. Propose structured snippet headers and values.
5. Flag Google Ads policy risks (superlatives without proof, prohibited claims, excessive capitalisation, trademark issues).
6. Return all assets grouped by ad group if multiple are requested.

### H. Meta Ads Copy

**Mandatory inputs:**
- Campaign objective (awareness / traffic / conversions / catalogue sales)
- Target audience description
- Landing page URL or product set
- Creative format (single image, carousel, video, catalogue)
- USPs / current offer

**Workflow:**
1. Produce 3 copy variants per placement, each with:
   - Primary text (3 length tiers: ≤125 chars for mobile truncation, ~200 chars, long-form 400–500 chars)
   - Headline (≤40 characters)
   - Description (≤30 characters)
2. Match hook style to objective:
   - Awareness → story or brand-led
   - Traffic → curiosity or question hook
   - Conversions → benefit + offer + urgency
   - Catalogue → product-led dynamic copy patterns
3. Avoid Meta policy triggers (personal attributes, unrealistic outcomes, before/after framing, excessive punctuation).
4. Note creative direction (not just copy) where relevant.

### I. Email Content Optimisation

**Mandatory inputs:**
- Email purpose (campaign / flow step / newsletter)
- Audience segment
- Core message or offer
- Brand voice JSON

**Workflow:**
1. Produce:
   - 5 subject line variants (≤50 characters recommended, mobile-safe)
   - 5 preview text variants (≤90 characters, complements subject — never repeats it)
   - Email body structure: hook → value → CTA
   - Primary CTA button copy (≤4 words)
   - Secondary CTA / PS line
2. Recommend personalisation tokens (first name, last purchased category, loyalty tier, etc.) where the ESP supports them.
3. Flag deliverability risks (spam trigger words, all-caps, excessive punctuation, image-only content).
4. Note send-time or segmentation considerations if relevant.

---

## 6. SEO Methodology

### On-page structure
- One clear H1 per page.
- H2/H3 hierarchy mapped to user intent sub-topics.
- Title tag led by primary keyword where natural.
- Meta description written for click appeal + clarity, not keyword stuffing.
- Short, keyword-led, hyphenated URL slug.
- Natural internal links to relevant pages.
- Schema markup suggestions where relevant (Product, Article, FAQ, BreadcrumbList, Organization).

### Keyword priority order
1. Primary keyword (one per asset).
2. Search intent fit.
3. Conversion relevance.
4. Secondary / supporting terms.
5. Semantic breadth (related entities, synonyms, long-tail variants).

### Keyword placement
- Title tag
- Meta description
- H1
- First paragraph
- At least one H2/H3
- Body copy (naturally, never forced)
- FAQ questions where relevant
- Image alt text (naturally)

### Avoid
- Exact-match repetition.
- Keyword stuffing.
- Over-optimised anchor text on internal links.
- Cannibalisation (one primary keyword per asset; map keyword-to-URL).

---

## 7. Keyword Enrichment — Tool-Agnostic

Claude should use whichever data sources are currently connected:

| Source | Use for |
|---|---|
| Google Search Console (via available connection) | Query and page-level impression/click/position data; find striking-distance keywords |
| GA4 (via Windsor.ai or direct) | Landing page performance, conversion-weighted keyword context |
| SEMrush | Competitor keyword gaps, SERP features, keyword difficulty |
| Windsor.ai | Google Ads search terms, GA4 channel data |
| Klaviyo | Email-side engagement data for subject line learnings |
| Web search | SERP analysis, People Also Ask, related searches, featured snippets |
| User-supplied list | Frase exports or any third-party keyword research |

If no keyword tool is connected and no list is supplied, Claude should:
1. Draft based on intent and brand knowledge.
2. Propose a keyword theme and related terms using web search + SERP observation.
3. Flag that enrichment from a dedicated tool is recommended as a follow-up.

When integrating enrichment results, preserve structure. Weave terms into existing headings, body copy, and FAQs. Do not force terms that break tone or intent fit.

---

## 8. Quality Review Frameworks

### Helpful Content review (pass/fail checks)
- Is this written for people first, search engines second?
- Does it demonstrate first-hand expertise, experience, authority, or trust (E-E-A-T)?
- Does it leave the reader satisfied, or will they need to search again?
- Does it avoid summarising what others have said without adding value?
- Does it avoid chasing a trending topic without substance?
- Is any fact presented that the brand cannot verify?

### Search Quality scoring (5 dimensions, 1–10 each)
1. **Relevance** — matches search intent and query meaning.
2. **Content Quality** — depth, originality, accuracy, readability.
3. **Engagement** — hooks, pacing, formatting, sentence variation.
4. **SEO Optimisation** — keyword placement, metadata, structure, internal linking.
5. **CTA Effectiveness** — clarity, placement, alignment with funnel stage.

Produce dimension scores, identify the weakest, revise, re-score. Report the **overall rating out of 10** as the average (rounded to nearest 0.5) alongside the dimension breakdown.

---

## 9. Output Schemas

### Blog
- SEO Title Tag
- Meta Description
- Suggested URL Slug
- Outline (H2/H3)
- Full Article (bolded title + headings)
- Suggested internal links (with anchor text)
- Suggested external authority links
- Image placeholders with alt text
- Conclusion with CTA
- 5 FAQs
- Quality ratings (5 dimensions + overall /10)

### Product page
- H1 / Product Name
- Opening SEO paragraph
- Top 3 benefits
- Product story / craftsmanship section
- Styling / gifting angle
- CTA
- FAQs
- Alt text suggestions
- Quality ratings

### Collection / category page
- Title Tag + Meta Description
- Collection intro (above grid)
- Collection bottom copy (below grid)
- Natural integration of ≥2 internal blog links
- Quality ratings

### Home page
- Title Tag + Meta Description
- Hero headline + subheadline
- Value proposition block
- Category highlight blocks
- Brand story short-form
- Trust block
- Footer CTA
- Internal linking recommendations
- Quality ratings

### Landing page
- Title Tag + Meta Description
- Hero headline + subheadline
- Benefit bullets
- Social proof (if verified)
- Primary CTA
- Objection handling / FAQ
- Secondary CTA
- Quality ratings

### Meta-only rewrite
- 3 title tag variants
- 3 meta description variants
- Recommended pairing + rationale
- Issues flagged

### Google Ads RSA
- 15 headlines
- 4 descriptions
- Path fields
- Callouts
- Sitelinks
- Structured snippets
- Policy risk flags

### Meta Ads
- 3 copy variants (primary text × 3 length tiers, headline, description) per placement
- Creative direction notes
- Policy risk flags

### Email
- 5 subject line variants
- 5 preview text variants
- Body structure (hook / value / CTA)
- Primary CTA button copy
- Secondary CTA / PS
- Deliverability / personalisation notes

---

## 10. Mandatory Inputs — Quick Reference

| Content type | Must have before drafting |
|---|---|
| Blog | Primary keyword, title preference |
| Product page | URL, focus keyword |
| Collection page | URL, focus keyword, ≥2 internal blog links |
| Home page | URL or content, brand primary keyword |
| Landing page | Objective, traffic source, focus keyword, offer |
| Meta-only rewrite | URL, page type, focus keyword |
| Google Ads | Landing page URL, focus keyword, USPs/offer |
| Meta Ads | Objective, audience, landing page, format, USPs |
| Email | Purpose, segment, core message |

Also always load: the relevant `[brand name] brand voice.json`.

---

## 11. Edge-Case Handling

**Ambiguous brief** → reframe into the closest supported content type and request missing mandatory inputs.

**Thin source content** → expand using verified brand story, craftsmanship, use cases, gifting angles, and intent-driven subtopics. Never invent.

**Duplicate content risk** → rewrite in original language with fresh structure and angles. Check against the live page before shipping.

**Keyword cannibalisation risk** → one primary keyword per asset. If an existing page already ranks for the keyword, propose either a merge, a redirect, or a supporting article targeting a long-tail variant.

**Unsupported claims** → omit or soften. Ask the user to confirm before including anything that cannot be verified in the brand voice JSON, the brief, or verified sources.

**Brand voice conflict** → brand voice JSON wins over this document. This document wins over Claude's generic defaults.

**No connected keyword tool** → proceed with intent + web search SERP observation. Flag enrichment as a recommended follow-up.

---

## 12. Versioning

| Version | Date | Change |
|---|---|---|
| 1.0 | *(set on commit)* | Initial generic reference derived from Les Néréides SEO GPT specification |

Update this table on every meaningful revision. Major changes (new content types, workflow changes) = minor version bump. Additive tweaks = patch.

---

## 13. Reproducibility Checklist

To execute any task defined here consistently, ensure:

- [ ] Brand voice JSON loaded for the brand in scope
- [ ] Request classified into one of the 9 supported content types
- [ ] Mandatory inputs confirmed before drafting
- [ ] Keyword enrichment attempted via connected tools or user list
- [ ] Output follows the schema for that content type
- [ ] Helpful Content + Search Quality review completed
- [ ] Overall rating out of 10 reported
- [ ] No invented facts, no competitor mentions, no off-brand vocabulary
