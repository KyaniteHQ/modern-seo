# Structured Data

Google's AI optimization guide states explicitly: **structured data is not a requirement for appearing in generative AI features.** It remains a best practice because:

- It qualifies pages for rich results (stars, prices, FAQs in classical SERPs).
- It reduces ambiguity about what a page represents.
- It can feed Google Knowledge Graph signals for entities (organization, products, people).

Google's wording is exact: structured data "isn't required for generative AI search, and there's no special schema.org markup you need to add. However, it's a good idea to continue using it as part of your overall SEO strategy, as it helps with being eligible for rich results."

Don't sell it to the user as "the AI Overviews unlock." Treat it as ordinary SEO hygiene with rich-result upside.

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
| `Product` | Product detail pages | Price, availability, aggregateRating (only if you genuinely collect reviews) |
| `Service` | Service detail pages | Useful for service businesses (logistics, consulting, agencies) |
| `FAQPage` | Pages with a real FAQ section visible to users | Rich results deprecated 2026-05-07 (gov/health-only since 2023-09-14); markup harmless but yields no rich result |
| `HowTo` | Step-by-step instructional content | Rich results deprecated 2023-09-14; markup harmless but yields no rich result |
| `VideoObject` | Pages with embedded original video | thumbnailUrl, uploadDate, duration |

### Schema deprecation tracker

- **FAQPage**: Rich results removed 2026-05-07 (Search Console reports/test removed June 2026, API support removed August 2026). FAQ rich results had already been restricted to authoritative government and health sites since 2023-09-14; 2026-05-07 is the final complete removal — it was never universal. Markup remains valid schema.org and is harmless to keep, but do not implement it expecting a SERP rich result. ([developers.google.com/search/docs/appearance/structured-data/faqpage](https://developers.google.com/search/docs/appearance/structured-data/faqpage))
- **HowTo**: Rich results removed 2023-09-14. Markup remains valid schema.org and harmless to keep. The structured-data doc now redirects to the changelog.
- **QAPage**: Still supported and the recommended alternative for Q&A and forum content; gained new supported properties 2026-03-24. Pair with Discussion Forum markup for threaded forum pages.

The table above is a working subset. Google documents a broad gallery of around 26 rich-result types in its [search gallery](https://developers.google.com/search/docs/appearance/structured-data/search-gallery) — Article, Breadcrumb, Carousel, Course, Dataset, Discussion forum, Event, FAQ, Image metadata, Job posting, Local business, Math solver, Movie, Organization, Product, Profile page, Q&A, Recipe, Review snippet, Software app, Speakable, Subscription/paywalled content, Vacation rental, Video, and more. Pick the documented type that genuinely matches your content. Don't fabricate "AI" schema types — there is no such thing, and inventing one does nothing.

## Common mistakes

- **Markup that doesn't match the visible page.** Adding `FAQPage` schema without an actual visible FAQ violates Google's structured data guidelines and can trigger manual actions. The markup must reflect content the user sees.
- **`AggregateRating` without real reviews.** Inventing star ratings is a quick way to lose rich-result eligibility.
- **Fake `Review` snippets** for services or businesses with no genuine review surface.
- **Multiple conflicting `Organization` blocks** across templates. Define once, sitewide.
- **`BreadcrumbList` that disagrees with the URL structure.** The schema should match the actual page hierarchy.
- **Forgetting `inLanguage`** for non-English sites. Set it to the page's actual language (e.g., `"tr-TR"` for Turkish content).

## Labeling AI-generated content in structured data

If you use AI to generate product imagery or copy, labeling it is a compliance requirement, not optional decoration. Google's guidance on using generative AI content (updated 2025-12-10) sets concrete expectations for ecommerce:

- **AI-generated images** must include IPTC `DigitalSourceType` metadata with the value `TrainedAlgorithmicMedia`.
- **AI-generated product data** (title, description, attributes) "must be specified separately and labeled as AI-generated" — this is relevant for Merchant Center feeds, not just on-page markup.

The takeaway: the labeling is the requirement. Skipping it on AI-generated feed data or imagery is a policy problem, not a missed optimization.

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
