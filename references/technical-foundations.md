# Technical Foundations for AI Discovery

Generative AI features are powered by Google's standard crawling and indexing pipeline. If a page can't be crawled or isn't indexed, no amount of content quality or structured data will surface it in AI Overviews. Start here.

## Indexing eligibility

A page must clear all of these to be eligible for AI Overviews and AI Mode:

1. **Crawlable** — not blocked by `robots.txt`, no `noindex` directive, reachable via internal links from the indexed graph.
2. **Indexable** — returns a 200 status, no soft-404, canonical points to itself (or to a chosen canonical), not deduplicated against another URL.
3. **Snippet-eligible** — no `data-nosnippet` attribute on the main content, no `max-snippet:0` directive in `robots` meta. (AI Overviews use snippet-eligible content as ground material.)
4. **Has meaningful indexable content** — server-rendered or hydrated content Googlebot can actually read.

Quick checks: Google Search Console's URL Inspection tool, `curl -A "Googlebot" -L <url>` for headers, the live "Test Live URL" feature in GSC to see what Googlebot renders.

## Crawlability

- **`robots.txt`**: serve at `/robots.txt`, return 200, allow Googlebot to fetch CSS and JS (blocking these is a classic mistake — Google needs them to render the page).
- **XML sitemap**: list canonical URLs only, include `lastmod`, reference it from `robots.txt`, and submit in GSC.
- **Internal linking**: every important page should be reachable within a few clicks from the homepage. Orphan pages get crawled rarely.
- **Crawl budget** (large sites only): consolidate duplicates with canonicals, use `noindex` on thin/utility pages, return proper HTTP codes (301 for moves, 410 for deleted, never 200 for a "not found" page).

## JavaScript SEO

Googlebot can render JavaScript, but JS adds a second processing pass that can delay indexing and reveals more failure modes. Treat JS SEO with extra care:

- **Prefer server rendering** (SSR) or **static generation** (SSG) for primary content. In Next.js, that means rendering with `app/` server components or returning pre-built HTML — keep critical content out of client-only components.
- **Avoid blocking content on hydration**: if the user has to wait for `useEffect` to fetch content, Googlebot's first pass sees an empty shell.
- **Don't block JS or CSS in robots.txt** — that defeats rendering entirely.
- **Avoid client-side routing without `<a href>` fallbacks**: links must be real anchors with `href` attributes that yield the destination URL even without JS.
- **Test what Googlebot sees**: use GSC URL Inspection → "Test Live URL" → "View tested page" → HTML tab. If your hero copy isn't there, neither is Google's understanding of the page.

For Next.js specifically: most route handlers and server components produce indexable HTML by default. The traps are usually (a) content fetched inside `useEffect` in a client component, (b) misuse of `dynamic = 'force-dynamic'` without a stable representation, (c) modal-only content that has no canonical URL.

## Semantic HTML and readability

Google does not require perfectly valid HTML, but semantic markup helps both AI systems and screen readers identify what's primary versus chrome:

- One `<h1>` per page that names the topic.
- `<main>` wraps the primary content; site nav, footer, sidebars are outside it.
- Headings (`<h2>`, `<h3>`) form a real outline of the content, not a styling shortcut.
- Lists, tables, and `<figure>`/`<figcaption>` for the things they actually represent.
- `<article>` for self-contained content blocks (blog posts, product cards).
- Skip "div soup" when a real element exists. `<button>` for buttons, not `<div onclick>`.

## User experience and performance

- **Mobile**: site renders well on small screens. Google indexes mobile-first.
- **Core Web Vitals**: meet thresholds for LCP, INP, and CLS. Slow pages get less crawl, less rank, and less AI surfacing.
- **Main content is distinguishable**: avoid layouts where ads, nav, and chrome dominate above the fold and the actual content is buried.
- **No intrusive interstitials** on mobile (cookie banners are fine; full-screen popups before the user reads anything are not).

## Duplicate content

- Use **canonical URLs** to consolidate variants (with/without trailing slash, tracking params, sort orders).
- Avoid serving the same content under both `www` and apex, both `http` and `https`. Redirect to one canonical host.
- Faceted navigation often creates millions of near-duplicates; either canonicalize them or block crawling of the parameter combinations that don't add unique value.
- Reducing duplicate content preserves crawl budget and clarifies which version Google should rank.

## Next.js / React app-specific checklist

For an `app/`-router Next.js app:

- Every public route exports a `generateMetadata` (or static `metadata`) with `title`, `description`, `openGraph`, `twitter`, and `alternates.canonical`.
- A root `app/sitemap.ts` and `app/robots.ts` exist and are reachable (`/sitemap.xml`, `/robots.txt`).
- Image components use `next/image` with explicit `width`/`height` and meaningful `alt`.
- All anchor navigation uses `next/link` or `<a href>` — no `onClick` push-state navigation for primary links.
- Server components render the main content by default; client components are leaves, not roots.
- `metadataBase` is set so absolute URLs in `og:image` etc. resolve.
- 404 page returns a real 404 status (not 200 with a "not found" page).
