# E-commerce SEO Best Practices Reference

## Document metadata

| Field | Value |
|---|---|
| Version | 1.0 |
| Last updated | 2026-04-17 |
| Next scheduled update | Quarterly (via automation) |
| Scope | E-commerce SEO best practices for 2026 and beyond |
| Primary audience | Claude, when producing SEO content across Kris's four Shopify brands |
| Companion files | `seo-content-reference.md`, `[brand name] brand voice.json` |
| Source URLs | See Section 10 — all claims in this document are citation-numbered to a source |

## How Claude should use this document

Treat this as the authoritative best-practice layer above the workflows in `seo-content-reference.md`. When producing any SEO content, cross-check against the principles here. Cite the numbered source from Section 10 when making a specific technical or statistical claim that a user might question. Do not quote passages verbatim — paraphrase and apply.

When a best practice in this document conflicts with a brand voice JSON, brand voice wins on tone and vocabulary; this document wins on technical SEO and Core Web Vitals.

Shopify context: all four brands (Les Néréides Australia, Love Persimmon, Roberto Verino ANZ, Mr. Boho) operate on Shopify. Where a best practice is platform-specific, Shopify-relevant notes are called out inline.

---

## 1. The 2026 search landscape

Discovery has fragmented across traditional search, AI Overviews, visual search, social commerce, and answer engines. The single search box has been replaced by a multi-modal journey involving Google Lens, voice assistants, and generative AI platforms like ChatGPT, Gemini, and Perplexity [1]. A modern e-commerce site effectively functions as a structured data feed serving multiple endpoints, not just a visual interface [1].

There are no tricks for appearing in AI Overviews. The system prioritises pages that demonstrate fundamental health: clean indexing, strong page experience, and genuine topical authority [9]. The brands that win are those that build pages answering the complete buyer journey — from education to final technical specs to trust verification [5].

| Surface | Discovery mechanism | Primary technical requirement |
|---|---|---|
| Traditional search | Keywords and backlinks | Crawlability and indexing [10] |
| AI Overviews | Semantic RAG | Structured data and topical depth [1] |
| Visual search (Lens) | Image recognition and metadata | High-resolution, optimised imagery [8] |
| Social commerce | Algorithms and engagement | Shoppable video and UGC integration [13] |
| Answer engines | Direct entity matching | API-first architecture and schema [1] |

---

## 2. Technical infrastructure

### 2.1 URL design

- Descriptive, keyword-rich URL paths outperform random IDs or codes [6].
- Lowercase all paths. Uppercase and lowercase versions of the same URL create duplicate crawl requests [15].
- Keep URLs short, hyphenated, and human-readable.
- Never include session IDs or temporary parameters in internal links — they create crawl waste [4].

Shopify note: Shopify auto-generates `/products/`, `/collections/`, and `/blogs/` URL prefixes. These cannot be removed, but the handle (the part after the prefix) should be clean, short, and keyword-led.

### 2.2 Site hierarchy

- Follow a flat structure: Home → Top categories → Subcategories → Product pages [8].
- Every product page should be accessible within three clicks of the home page [2].
- Implement breadcrumb navigation with schema markup to clarify page relationships for users and crawlers [6].

### 2.3 Faceted navigation

Filters for size, colour, material, and price are the primary source of crawl waste [1]. A store with 1,000 products can theoretically generate millions of low-value filter-combination URLs [2].

The 2026 governance standard:
- Only broad category pages and high-demand specific filters should be indexed [2].
- Granular filters (narrow price ranges, obscure size combinations) should be canonicalised or blocked via `robots.txt` to prevent index bloat [1].
- Google does not use fragment identifiers (`#`) for indexing — so `/product#blue` and `/product#red` are the same page, which can be leveraged to prevent duplicate indexing of variants while maintaining user experience [15].

Shopify note: Shopify's default collection filter URLs (`?filter.v.option.size=M`) should have `robots.txt` rules or `noindex` logic applied via theme code to prevent bloat. The theme's `robots.txt.liquid` file allows per-brand customisation.

### 2.4 URL strategy quick reference

| Component | Standard | Rationale |
|---|---|---|
| Path naming | Descriptive keywords (`/mens-running-shoes`) | Thematic relevance aids ranking [4] |
| Variant handling | Unique URLs for distinct variant pages | Helps Google Search and Shopping identify specific offers [15] |
| Query parameters | `?key=value` format | Communicates structure to Googlebot [2] |
| Internal links | Persistent, long-term URLs | Avoids session-ID duplication [4] |
| Redirection | Direct paths, no chains | Preserves crawl budget and load speed [2] |

### 2.5 Bot governance and `robots.txt`

In 2026, `robots.txt` is a strategic governance document for managing AI agent access [1]. The distinction now matters between:

- **Retrieval bots** (surface content in AI search results) — generally keep allowed.
- **Training bots** (ingest data to train foundational models) — optional to block if the brand does not want its data used for model training.

`OAI-SearchBot` is crucial for ChatGPT search visibility; `GPTBot` can be optionally blocked without immediate traffic impact [1].

### 2.6 Out-of-stock handling

"Soft 404s" — pages that return `200 OK` but show "unavailable" messaging — are treated as thin content and degrade quality signals [1]. Handling:

- **Temporarily out of stock**: keep the page live with internal links to similar items.
- **Permanently removed**: return `404` or `410`.

Shopify note: Shopify's default behaviour for unavailable products keeps the page live with an "unavailable" button. Ensure out-of-stock pages still have strong internal links (related products, collection link, category link) to avoid soft-404 classification.

---

## 3. Performance and Core Web Vitals

Website speed is a direct commercial driver, not a peripheral metric [17].

### 3.1 Core Web Vitals thresholds (Google, 2026)

| Metric | Measures | Good threshold | Poor threshold |
|---|---|---|---|
| **LCP** (Largest Contentful Paint) | Loading performance | ≤ 2.5 seconds | > 4 seconds |
| **INP** (Interaction to Next Paint) | Responsiveness | ≤ 200 milliseconds | > 500 milliseconds |
| **CLS** (Cumulative Layout Shift) | Visual stability | ≤ 0.1 | > 0.25 |

INP replaced FID (First Input Delay) as a Core Web Vital in March 2024 [18]. All three are measured at the 75th percentile of real user data — 75% of page loads must meet the "good" threshold for a page to pass overall [11].

### 3.2 The financial impact of latency

- A one-second delay in page load can reduce conversion rates by approximately 7% [17].
- A three-second delay can result in a significant conversion reduction; specific percentages vary by study but consistently fall in the 20–30% range [17].
- Over half of mobile visitors abandon a page that takes more than three seconds to load [17].
- Small load-time improvements compound — even fractional-second gains show measurable lift in conversions and AOV [17].

The interaction between performance and visibility is symbiotic: sites that pass Core Web Vitals see higher rankings, which drives more traffic, which produces more sales signals [17]. High bounce rates from slow pages send negative signals to the algorithm [17].

### 3.3 Optimisation strategies

**For LCP:**
- Identify whether the main product image or hero banner is the LCP element [21].
- Convert images to WebP or AVIF [1].
- Use `fetchpriority="high"` on the LCP image [1].
- Preload critical resources (hero images, critical CSS, web fonts) [19].

**For CLS:**
- Always specify `width` and `height` attributes on images and video embeds [3].
- Reserve space for ads, embeds, and dynamically injected content.
- Avoid fonts that cause text reflow after load.

**For INP:**
- Defer non-critical JavaScript [12].
- Audit third-party scripts (chat, analytics, ad pixels) — these are the most common INP killers on e-commerce [9].
- Optimise complex event handlers for `add to cart`, filter changes, and variant selection [12].

**Architecture:**
- Incremental Static Regeneration (ISR) is the preferred 2026 rendering model — static speed with dynamic freshness [1].

Shopify note: Shopify's Liquid templates render server-side, which helps LCP. The common INP problems on Shopify are: heavy third-party apps, cart drawer scripts, and non-optimised theme JavaScript. Audit installed apps regularly and uninstall unused ones.

---

## 4. Semantic architecture and schema

Structured data is the primary language of e-commerce in 2026 [1]. Sites with comprehensive schema see meaningfully better performance in both traditional and voice search [16].

### 4.1 Essential schema types

| Schema type | Core properties | Strategic value |
|---|---|---|
| **Product** | `name`, `description`, `brand`, `sku`, `material`, `color` | Foundation for all e-commerce rich results [16] |
| **Offer** | `price`, `priceCurrency`, `availability`, `priceValidUntil` | Enables price-drop alerts and accurate merchant listings [1] |
| **Review / AggregateRating** | `reviewBody`, `author`, `datePublished`, `reviewRating` | Star ratings in SERPs, significant CTR lift [6] |
| **Organization** | `legalName`, `logo`, `contactPoint`, `address` | Establishes brand as a verifiable Knowledge Graph entity [16] |
| **BreadcrumbList** | `itemListElement`, `position`, `name`, `item` | Clarifies site structure and improves navigational clicks [6] |
| **FAQPage** | `Question`, `Answer` | Can appear as rich snippets; reinforces topical authority |
| **Article / BlogPosting** | `headline`, `author`, `datePublished`, `image` | Required for article rich results |

### 4.2 Merchant listings

Merchant listing rich results are transactional and require specific schema properties: detailed pricing, availability (`InStock` / `OutOfStock`), and clear return policies [1].

### 4.3 Schema drift

"Schema drift" is the discrepancy between data in structured markup and what's visible on the rendered page [1]. If prices, stock levels, or ratings differ between schema and visible content, pages can be penalised or excluded from rich results [1]. Automate schema-to-page consistency checks.

Shopify note: Shopify themes vary widely in schema coverage. Dawn and most modern themes include Product, Offer, and BreadcrumbList schema by default. Review schema typically requires a review app (Yotpo, Okendo, Judge.me) which injects AggregateRating. Verify with Google's Rich Results Test after any theme or app change.

### 4.4 Entity recognition

Build topical authority around recognisable entities. Connect brand, products, and collections to broader entity graphs via:
- Consistent branding and organisational schema across all properties
- External backlinks from authoritative sources within the category
- Clear category taxonomy that maps to Google's understanding of the vertical
- Wikipedia / Wikidata presence for the brand where justified

---

## 5. Helpful content and E-E-A-T

In 2026, the "new SEO" for AI surfaces is a return to fundamentals: clear structure, definitive expertise, and comprehensive answers to shopping intent [9].

### 5.1 E-E-A-T principles

- **Experience**: demonstrate first-hand use or expertise with the product category.
- **Expertise**: surface author credentials, specialist knowledge, genuine craft detail.
- **Authoritativeness**: link to and be linked from authoritative sources in the vertical.
- **Trustworthiness**: verifiable claims, clear policies, transparent contact information.

### 5.2 Content depth for product discovery

Top-performing product and collection pages in 2026 answer the full buyer journey in one place:
- What the product is and does
- How it compares to alternatives (without naming competitors)
- Use cases and styling or application scenarios
- Materials, craft, or ingredient details
- Sizing, compatibility, or fit guidance
- FAQ covering the top 5 pre-purchase objections
- Trust signals (reviews, returns, warranty, certifications if verified)

### 5.3 Writing for humans first

- Short sentences, scannable structure.
- Clear H2/H3 hierarchy aligned to search intent.
- Bullets and tables for comparative information.
- Lead with benefit, support with detail.
- Avoid keyword stuffing — semantic coverage beats repetition.

---

## 6. International and multi-market SEO

For brands operating across markets (Les Néréides Australia, Roberto Verino ANZ, Mr. Boho global), `hreflang` and localised URL structure are essential [27].

### 6.1 hreflang requirements

- **Bidirectional confirmation**: if the English page points to the Spanish version, the Spanish version must point back [1].
- **Self-referencing**: every page must include an `hreflang` tag pointing to itself [1].
- **`x-default`**: critical in 2026 as a fallback for users whose language isn't specifically targeted — typically points to a language selector or global version [29].

### 6.2 URL strategy for international

Three approved patterns:
- **Country code top-level domain** (ccTLD): `lesnereides.fr`, `lesnereides.com.au` — strongest geo-signal, most expensive to maintain.
- **Subdomain**: `au.brand.com` — clear geo-signal, moderate maintenance.
- **Subdirectory**: `brand.com/au/` — cheapest to maintain, weakest geo-signal but works well with strong hreflang implementation.

Shopify note: Shopify Markets enables subdirectory-based localisation with automatic `hreflang` tag injection on a single domain. This is the recommended 2026 pattern for multi-market Shopify stores.

---

## 7. Visual and social commerce

### 7.1 Short-form video

Static product imagery alone is no longer sufficient [13]. Short-form videos (typically 15–60 seconds) excel at highlighting features, solving problems, and demonstrating lifestyle use [13]. Most major platforms (TikTok Shop, Instagram Shopping, YouTube Shopping) now support direct product tagging within video [13].

### 7.2 User-generated content (UGC)

UGC is considered more impactful than professionally shot imagery by a majority of e-commerce marketers, driven by consumer trust patterns favouring peer content over brand content [14]. Best practice in 2026:

- Shoppable UGC galleries on product and collection pages [14].
- Dynamic ranking based on conversion probability and authenticity, not chronology [14].
- Automated rights management workflows for legal consent [14].
- Micro-creators (smaller followings, higher engagement) often outperform mega-influencers for niche audiences [14].

### 7.3 Live shopping and video commerce

Live streams create urgency and direct connection, functioning as virtual storefronts with real-time interaction [13]. Repurposing customer videos into on-product-page highlights shows meaningful conversion lift [14].

Shopify note: Shopify supports Instagram Shopping, TikTok Shop, and YouTube Shopping integrations natively. UGC apps include Pixlee, Foursixty (for Instagram), and Okendo Reviews with photo/video support.

---

## 8. UX and CRO benchmarks

### 8.1 2026 conversion benchmarks

The global e-commerce average conversion rate sits in the 2–3% range, with significant variation by vertical and price point [31]. A reliable rule: the higher the average order value, the lower the typical conversion rate.

| Vertical | Conversion rate context |
|---|---|
| Food & Beverage | Highest average — typically 3–5%+ [31] |
| Health & Beauty | Above average — typically 2–4% [20] |
| Fashion & Apparel | Around average — typically 1.5–2.5% [31] |
| Consumer Electronics | Slightly below average — 1.5–2% [20] |
| Home & Garden | Slightly below average — 1.5–2% [20] |
| Luxury & Jewellery | Below average — typically 0.8–1.5% [31] |

*Specific benchmark percentages should be verified on each automation update — these ranges represent consistent industry observation as of early 2026.*

### 8.2 Device patterns

Mobile now accounts for the majority of sessions in most verticals [31]. While mobile conversion rates historically trailed desktop, convergence has narrowed the gap [33]. Desktop still generates higher AOV and a disproportionate share of revenue relative to traffic [33].

### 8.3 Product page UX

Baymard Institute research shows most e-commerce product pages underperform on UX fundamentals [35]. Common failure points:

- **Dropdown menus for size**: hide availability, create wasted effort. Visible swatches are preferred [35].
- **Missing "in scale" imagery**: showing the product next to a known reference object or human model helps users physically evaluate [35].
- **Poor variant switching**: should update imagery, price, and availability without page reload.
- **Weak social proof placement**: reviews should be visible near price/CTA, not only at the bottom.

### 8.4 Mobile optimisation

- Design for the thumb zone (lower two-thirds of the screen) [12].
- Tap targets at least 44×44 pixels [12].
- Single-column flows for key conversion paths [12].
- Digital wallets (Apple Pay, Google Pay, Shop Pay) and Buy Now Pay Later options are now baseline expectations [26].

### 8.5 Checkout

- **Forced account creation is the single biggest checkout killer** [20].
- Offer guest checkout by default; offer account creation after order is placed [33].
- Minimise form fields; use address auto-complete.
- Show shipping cost and delivery date early — hidden shipping cost is a top abandonment reason.
- Progress indicators on multi-step checkouts reduce drop-off.

Shopify note: Shopify Checkout already handles most of these well. Shop Pay accounts sit above guest checkout, which is optimal. Custom checkout modifications are only available on Shopify Plus.

---

## 9. Trusted sources for ongoing learning

Primary technical authorities (cite first when possible):

- **Google Search Central** ([developers.google.com/search](https://developers.google.com/search)) — official documentation; the ground truth [10].
- **Schema.org** ([schema.org](https://schema.org)) — non-negotiable standard for structured data [11].
- **web.dev** ([web.dev](https://web.dev)) — Google's performance and Core Web Vitals reference.

Research and benchmarks:

- **Baymard Institute** ([baymard.com](https://baymard.com)) — UX research gold standard, hundreds of thousands of hours of testing [35].

Data-driven SEO practitioners:

- **Ahrefs Blog**, **Moz Blog** — large-dataset SEO research [36].
- **Search Engine Land** — industry news and algorithm updates [8].
- **Search Engine Journal** — breaking news and tactical guides.

Specialist e-commerce:

- **Shopify Blog** — platform-specific best practice [6].
- **Klaviyo Blog** — retention, email, and loyalty [37].

Practitioners worth following:

- Aleyda Solis (SEOFOMO) — forensic algorithm analysis [11].
- Glenn Gabe (GSQi) — technical SEO and algorithm updates [11].
- Wil Reynolds (Seer Interactive) — SEO-to-business connection [11].

---

## 10. Source URLs

All claims in this document reference the following sources, numbered for inline citation.

| # | Title | URL |
|---|---|---|
| 1 | Full Technical SEO Checklist: The 2026 Guide — Yotpo | https://www.yotpo.com/blog/full-technical-seo-checklist/ |
| 2 | Technical SEO Guide: The 2026 Audit & Strategy Framework — Outpace | https://outpaceseo.com/article/technical-seo/ |
| 3 | Google Search Central: The Complete Guide — BlueGlass | https://blueglassinsights.com/article/google-search-central-complete-guide-seo-documentation |
| 4 | The Ecommerce SEO Guide for Scalable Growth 2026 — ResultFirst | https://www.resultfirst.com/blog/ecommerce-seo/ecommerce-seo-guide/ |
| 5 | Biggest Change for E-Commerce SEO 2026 — Reddit r/seogrowth | https://www.reddit.com/r/seogrowth/comments/1o4cgnk/biggest_change_for_ecommerce_seo_for_2026/ |
| 6 | E-commerce SEO Complete Guide 2026 — W3Era | https://www.w3era.com/blog/seo/ecommerce-seo-complete-guide/ |
| 7 | eCommerce SEO 2026: Proven Strategies — Commerce Pundit | https://www.commercepundit.com/blog/seo-for-ecommerce-what-actually-works-in-2026/ |
| 8 | E-commerce SEO guide: Google's new documentation — Search Engine Land | https://searchengineland.com/ecommerce-seo-guide-new-documentation-from-google-374788 |
| 9 | Ecommerce SEO 2026: Best Practices and Strategy — Neotype | https://neotype.ai/ecommerce-seo-best-practices-and-strategy/ |
| 10 | Google Search Essentials — HubSpot | https://blog.hubspot.com/marketing/webmaster-guidelines |
| 11 | Top SEO Blogs & Resources 2026 — SEO Mechanic | https://seomechanic.com/blog/top-seo-blogs-and-resources |
| 12 | eCommerce Web Design Best Practices 2026 — Shift8 | https://shift8web.ca/ecommerce-web-design-best-practices-in-2026/ |
| 13 | Best Social Media Strategies for E-Commerce 2026 — MoDuet | https://moduet.com/the-best-social-media-strategies-for-e-commerce-in-2026/ |
| 14 | UGC for eCommerce 2026 — CS-Cart | https://www.cs-cart.com/blog/ugc-ecommerce/ |
| 15 | Ecommerce URL Structure Best Practices — Google Search Central | https://developers.google.com/search/docs/specialty/ecommerce/designing-a-url-structure-for-ecommerce-sites |
| 16 | E-commerce Schema Markup Guide 2026 — Koanthic | https://koanthic.com/en/e-commerce-schema-markup-complete-guide-examples-2026/ |
| 17 | 1-Second Delay Cuts Conversions by 7% (2026 Data) — Wiro Agency | https://www.wiro.agency/blog/how-a-1-second-delay-costs-you-a-7-drop-in-conversions |
| 18 | Why Core Web Vitals Still Matter in 2026 — Interact Marketing | https://www.interactmarketing.com/why-core-web-vitals-still-matter-more-than-you-think-in-2026/ |
| 19 | Core Web Vitals Optimization Guide 2026 — Sky SEO Digital | https://skyseodigital.com/core-web-vitals-optimization-complete-guide-for-2026/ |
| 20 | eCommerce Conversion Rates 2026 — Network Solutions | https://www.networksolutions.com/blog/ecommerce-conversion-rate/ |
| 21 | Core Web Vitals for E-commerce 2026 — W3Era | https://www.w3era.com/blog/seo/core-web-vitals-ecommerce-fix/ |
| 22 | Core Web Vitals and SEO 2026 — Weblogic.ie | https://weblogic.ie/blog/core-web-vitals-explained/ |
| 23 | SEO Best Practices for Ecommerce Sites — Google Search Central | https://developers.google.com/search/docs/specialty/ecommerce |
| 24 | Schema Markup: The Complete Guide 2026 — We Are TG | https://www.wearetg.com/blog/schema-markup/ |
| 25 | Schema Markup in 2026: Critical for SERP Visibility — ALM Corp | https://almcorp.com/blog/schema-markup-detailed-guide-2026-serp-visibility/ |
| 26 | Most Effective eCommerce Marketing Strategies 2026 — Sagemind | https://sagemindmarketing.com/the-most-effective-ecommerce-marketing-strategies-for-2026/ |
| 27 | International SEO: Best Practices 2026 — JetOctopus | https://jetoctopus.com/international-seo/ |
| 28 | International SEO & GEO 2026 — Elementor | https://elementor.com/blog/international-seo-geo-best-practices-strategy-in-year/ |
| 29 | Hreflang Implementation Guide — LinkGraph | https://www.linkgraph.com/blog/hreflang-implementation-guide/ |
| 30 | Hreflang Tags Ultimate 2026 Guide — ClickRank | https://www.clickrank.ai/hreflang-tags-complete-guide/ |
| 31 | Ecommerce Conversion Rate by Industry 2026 — Convertibles | https://convertibles.dev/blogs/optimization/increase-ecommerce-conversion-rate |
| 32 | Average Ecommerce Conversion Rate 2026 — Qualimero | https://qualimero.com/en/blog/average-ecommerce-conversion-rate |
| 33 | Average Ecommerce Conversion Rate 2026 — Red Stag Fulfillment | https://redstagfulfillment.com/average-conversion-rate-for-ecommerce/ |
| 34 | Ecommerce Conversion Rate Benchmarks 2026 Shopify — Blend Commerce | https://blendcommerce.com/blogs/shopify/ecommerce-conversion-rate-benchmarks-2026 |
| 35 | Product Page UX Best Practices 2026 — Baymard Institute | https://baymard.com/blog/current-state-ecommerce-product-page-ux |
| 36 | Top SEO Blogs 2025–2026 — Primary Position | https://primaryposition.com/blog/top-seo-blogs/ |
| 37 | Best Ecommerce Blogs 2026 — Let's Talk Shop | https://www.letstalkshop.com/blog/best-ecommerce-blogs-worth-reading-in-2026 |
| 38 | Baymard 2025 Review and 2026 Roadmap — Baymard | https://baymard.com/blog/year-in-review-2025-and-2026-roadmap |
| 39 | UX-Ray Progress 2026 — Baymard | https://baymard.com/blog/ux-ray-progress-2026 |
| 40 | Ecommerce UX Research Articles — Baymard | https://baymard.com/blog |
| 41 | Best Digital Marketing Blogs 2026 — Digital Authority | https://www.digitalauthority.me/resources/best-digital-marketing-blogs-to-follow-in-2026/ |
| 42 | Complete Guide to Ecommerce SEO 2026 — DebugBear | https://www.debugbear.com/blog/ecommerce-website-seo |

---

## 11. Automation update instructions

This document is designed for periodic automated refresh. The automation should:

1. **Re-fetch all source URLs** in Section 10. Flag any that return 404 or have changed substantially.
2. **Refresh statistics** where sources have updated — especially conversion rate benchmarks (Section 8.1), Core Web Vitals thresholds (Section 3.1 — unlikely to change but check Google's official documentation), and any newly published 2026 or 2027 studies.
3. **Add new sources** for emerging topics — particularly anything relating to AI Overviews, answer engines, and new search surfaces.
4. **Archive deprecated practices** with a note and date rather than silently removing (helps track what changed and why).
5. **Update document metadata** at the top: increment version (1.0 → 1.1 for additions; → 2.0 for structural changes), update `Last updated` date.
6. **Log changes in the Changelog below.**

### Search queries for refresh

Good starting queries for automated research:
- "ecommerce SEO best practices {year}"
- "Core Web Vitals thresholds {year}"
- "Google AI Overviews ecommerce"
- "ecommerce conversion rate benchmarks {year}"
- "Shopify SEO best practices {year}"
- "schema markup ecommerce {year}"
- "Baymard product page research"

### Priority sources for the refresh

Prioritise official Google sources (Google Search Central, web.dev), Baymard Institute, and major platform documentation (Shopify) over agency blogs. Agency content is useful for scanning themes but should not be the sole source for specific statistics.

---

## 12. Changelog

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-04-17 | Initial optimised reference document. Structured from the raw 2026 research report with base64-embedded statistic images stripped, Core Web Vitals values confirmed from Google's official documentation, conversion rate benchmarks expressed as qualitative ranges pending automated refresh with exact figures, Shopify-specific context added inline throughout, source URLs consolidated into a numbered reference table, and automation update instructions added. |
