# Structured Data

Google's AI optimization guide states explicitly: **structured data is not a requirement for appearing in generative AI features.** It remains a best practice because:

- It qualifies pages for rich results (stars, prices, FAQs in classical SERPs).
- It reduces ambiguity about what a page represents.
- It can feed Google Knowledge Graph signals for entities (organization, products, people).

Don't sell it to the user as "the AI Overviews unlock." Treat it as ordinary SEO hygiene with rich-result upside.

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
| `FAQPage` | Pages with a real FAQ section visible to users | Don't fabricate Q&As just for markup |
| `HowTo` | Step-by-step instructional content | Steps must match the visible content |
| `VideoObject` | Pages with embedded original video | thumbnailUrl, uploadDate, duration |

## Common mistakes

- **Markup that doesn't match the visible page.** Adding `FAQPage` schema without an actual visible FAQ violates Google's structured data guidelines and can trigger manual actions. The markup must reflect content the user sees.
- **`AggregateRating` without real reviews.** Inventing star ratings is a quick way to lose rich-result eligibility.
- **Fake `Review` snippets** for services or businesses with no genuine review surface.
- **Multiple conflicting `Organization` blocks** across templates. Define once, sitewide.
- **`BreadcrumbList` that disagrees with the URL structure.** The schema should match the actual page hierarchy.
- **Forgetting `inLanguage`** for non-English sites. Set it to the page's actual language (e.g., `"tr-TR"` for Turkish content).

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
