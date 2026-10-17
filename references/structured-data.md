# Structured Data

Google's AI optimization guide states explicitly: **structured data is not a requirement for appearing in generative AI features.** It remains a best practice because:

- It qualifies pages for rich results (stars, prices, FAQs in classical SERPs).
- It reduces ambiguity about what a page represents.
- It can feed Google Knowledge Graph signals for entities (organization, products, people).

Google's wording is exact (AI optimization guide, updated 2026-07-10): structured data "isn't required for generative AI search, and there's no special schema.org markup you need to add. However, it's a good idea to continue using it as part of your overall SEO strategy, as it helps with being eligible for rich results on Google Search."

Don't sell it to the user as "the AI Overviews unlock." Treat it as ordinary SEO hygiene with rich-result upside.

Google's AI features page (updated 2025-12-10) lists "Making sure your structured data matches the visible text on the page" among the existing SEO fundamentals that "continue to be worthwhile" for AI Overviews and AI Mode. No Google document says markup is passed to those features as context (checked 2026-10), so don't claim it is. Markup that contradicts the page can cost rich-result eligibility.

A 2026-05-11 Ahrefs difference-in-differences study (~1,885 pages already cited in AI answers) found that adding JSON-LD produced **no positive AI-citation uplift** on any platform; the AI Overviews arm trended slightly negative. This is a single-vendor, directional study, but it consistently supports Google's stated position. Structured data's real value remains rich-result eligibility and entity disambiguation — not AI-citation frequency.

## Use JSON-LD

Use JSON-LD in a `<script type="application/ld+json">` block, ideally in the `<head>`. JSON-LD is easier to maintain than microdata or RDFa and is what Google recommends.

In Next.js (`app/` router), the cleanest pattern is a small component that returns the script tag, called from server components:

```tsx
export function JsonLd({ data }: { data: object }) {
  return (
    <script
      type="application/ld+json"
      dangerouslySetInnerHTML={{ __html: JSON.stringify(data) }}
    />
  );
}
```

## Schemas worth implementing

Pick by content type. Don't add schemas for content types that don't match.

| Schema | Use it on | Notes |
|---|---|---|
| `Organization` | Sitewide (homepage, footer, or `app/layout.tsx`) | Name, logo, URL, sameAs (social profiles), contactPoint |
| `LocalBusiness` (or subtype) | Local services / brick-and-mortar | Address, phone, hours, geo, area served. For logistics: `MovingCompany`, `AutoRepair`, etc. when applicable |
| `BreadcrumbList` | All non-home pages with a breadcrumb path | One per page |
| `Article` / `BlogPosting` | Editorial content | Headline, datePublished, dateModified, author, image |
| `Product` | Product detail pages | Price, availability, aggregateRating (only if you genuinely collect reviews). Optional `category` (documented 2026-07-07) mirrors the Merchant Center feed |
| `Service` | Service detail pages | Useful for service businesses (logistics, consulting, agencies) |
| `FAQPage` | Pages with a real FAQ section visible to users | Rich results deprecated 2026-05-07 (gov/health-only since 2023-09-14); markup harmless but yields no rich result |
| `HowTo` | Step-by-step instructional content | Rich results deprecated 2023-09-14; markup harmless but yields no rich result |
| `VideoObject` | Pages with embedded original video | Required: name, thumbnailUrl, uploadDate. Recommended `creator` (Person or Organization, documented 2026-09-24) attributes the video; `interactionStatistic` covers watch, like, comment, and share counts |

### Schema deprecation tracker

- **FAQPage**: Rich results removed 2026-05-07; docs removed 2026-06-15. As of 2026-10, FAQ is gone from Search Console's rich result reports and the Rich Results Test. FAQ rich results had already been restricted to authoritative government and health sites since 2023-09-14; 2026-05-07 is the final complete removal — it was never universal. Markup remains valid schema.org and is harmless to keep, but do not implement it expecting a SERP rich result. ([developers.google.com/search/updates#removing-faq-rich-result](https://developers.google.com/search/updates#removing-faq-rich-result); the old `/structured-data/faqpage` URL now redirects there)
- **HowTo**: Rich results removed 2023-09-14. Markup remains valid schema.org and harmless to keep. The structured-data doc now redirects to the changelog.
- **QAPage**: Still supported and the recommended alternative for Q&A and forum content; gained new supported properties 2026-03-24. Pair with Discussion Forum markup for threaded forum pages.

The table above is a working subset. Google documents a broad gallery of 25 rich-result types in its [search gallery](https://developers.google.com/search/docs/appearance/structured-data/search-gallery) (updated 2026-10-16; FAQ is no longer listed) — Article, Breadcrumb, Carousel, Course, Dataset, Discussion forum, Event, Image metadata, Job posting, Local business, Math solver, Movie, Organization, Product, Profile page, Q&A, Recipe, Review snippet, Software app, Speakable, Subscription/paywalled content, Vacation rental, Video, and more. Pick the documented type that genuinely matches your content. Don't fabricate "AI" schema types — there is no such thing, and inventing one does nothing.

## Common mistakes

- **Markup that doesn't match the visible page.** Adding `FAQPage` schema without an actual visible FAQ violates Google's structured data guidelines and can trigger manual actions. The markup must reflect content the user sees.
- **`AggregateRating` without real reviews.** Inventing star ratings is a quick way to lose rich-result eligibility.
- **Stale sale dates in `Product` markup.** Google: "Your listing may not display if the priceValidUntil property indicates a past date" (merchant listing, updated 2026-09-08). Update or remove sale dates when the sale ends.
- **Fake `Review` snippets** for services or businesses with no genuine review surface.
- **Multiple conflicting `Organization` blocks** across templates. Define once, sitewide.
- **`BreadcrumbList` that disagrees with the URL structure.** The schema should match the actual page hierarchy. Each non-final item's `item` must be a real page URL — Google defines it as *"the URL to the webpage that represents the breadcrumb"* — so use the canonical page path, not a fragment anchor like `/#section`. (Google doesn't document the fragment-URL failure mode, so treat "the rich result drops" as practitioner caution, not a stated rule.)
- **`LocalBusiness` without a `PostalAddress`, expecting a rich result.** Google lists `address` as a **required** property for `LocalBusiness` (and subtypes like `MovingCompany`) rich-result eligibility, and `areaServed` is a schema.org `Organization` property Google doesn't treat as a supported `LocalBusiness` field. A service-area business with no public address won't earn the local rich result from `LocalBusiness` markup — supply a registered/mailing address, or use `Organization` + `areaServed` instead.
- **Forgetting `inLanguage`** for non-English sites. Set it to the page's actual language (e.g., `"tr-TR"` for Turkish content).

## Labeling AI-generated content in structured data

If you use AI to generate product imagery or copy, labeling it is a compliance requirement, not optional decoration. Google's guidance on using generative AI content (updated 2026-10-01) sets concrete expectations for ecommerce:

- **AI-generated images** must include IPTC `DigitalSourceType` metadata with the value `TrainedAlgorithmicMedia`.
- **AI-generated product data** (title, description, attributes) "must be specified separately and labeled as AI-generated" — this is relevant for Merchant Center feeds, not just on-page markup.

The takeaway: the labeling is the requirement. Skipping it on AI-generated feed data or imagery is a policy problem, not a missed optimization.

The same guide's human fact-check rule "also applies to metadata like `<title>` elements, meta description elements, structured data, and alternate texts for images, which can appear in Search results," so fact-check generated JSON-LD too.

## Validation

- **Schema.org Validator**: https://validator.schema.org — checks structural validity.
- **Google Rich Results Test**: https://search.google.com/test/rich-results — checks Google-specific eligibility for rich results.
- **GSC's "Enhancements" reports**: shows live coverage and errors for each schema type Google detects on your site.

When debugging "why doesn't my schema show up": first verify it's actually rendered in the served HTML (not injected client-side after hydration — Google's first pass may miss it).

## When *not* to bother

- **Content types Google doesn't reward with rich results.** Most marketing pages don't gain rich results from generic `WebPage` schema. The effort is rarely worth it on every page.
- **Small sites with one product line.** Get `Organization` and `LocalBusiness` (if applicable) right and move on. Don't drown the project in JSON-LD for marginal lift.
- **Heavily duplicated content.** Fix the duplication first; structured data won't rescue a thin page.

## What to skip entirely

- `llms.txt` is not structured data and Google does not consume it (see [mythbusting.md](mythbusting.md)).
- "AI-specific" schema vocabularies you'll see promoted by SEO vendors. Use schema.org types Google documents; ignore the rest.
