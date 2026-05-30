# Content Strategy: Non-Commodity Content

The single biggest lever for visibility in both classical and AI search is creating content that **cannot be easily replicated by an AI model**. This is also the lever most teams ignore.

## The commodity test

Ask one question of any page: *if you removed the author, would anything remain that isn't already on the open web?*

If the answer is no, the page is commodity content. AI systems are trained on the open web and can already produce that material. Google's ranking systems prioritize sources that stand out, so commodity pages lose by definition.

Examples:

| Commodity | Non-commodity |
|---|---|
| "7 Tips for First-Time Homebuyers" | "Why We Waived the Inspection & Saved Money" |
| "What is Cloud Computing?" | "We Migrated 80 TB to S3 — Here's the Bandwidth Bill" |
| "Benefits of International Shipping" | "Three Mistakes That Cost Us $12k on Our First Cross-Border Container" |
| "How to Choose a Logistics Partner" | "Why We Stopped Using Door-to-Door Service for Heavy Industrial Loads" |

The non-commodity version always has: a specific situation, a specific actor (we / I), a specific number or detail, and an outcome that surprised or cost something.

## Sources of non-commodity content

Even teams that "don't have unique content" almost always do — they just haven't written it down. Common sources:

- **Operational specifics** — actual procedures, timelines, equipment, vehicle types, route choices, dock procedures, container handling steps.
- **Decisions and tradeoffs** — when you chose one approach over another and what made the difference (faster vs cheaper, sea vs air, owned vs subcontracted).
- **Failures and corrections** — what went wrong, what it cost, what you changed.
- **Numbers from the inside** — average delivery time on a specific route, typical damage rate by mode, real fuel-surcharge math.
- **Customer-specific stories** — anonymized but concrete (a 40ft container of glass from Izmir to Hamburg, what went into the quote, how the route was chosen).
- **Photos of your actual operation** — trucks, warehouses, packing teams, signed paperwork. Stock imagery is commodity by definition.

## E-E-A-T: trust is the core

Google evaluates content quality through **E-E-A-T**: Experience, Expertise, Authoritativeness, Trustworthiness ("Creating helpful, people-first content", updated 2025-12-10). The hierarchy matters:

> Of these aspects, trust is most important. The others contribute to trust, but content doesn't necessarily have to demonstrate all of them.

E-E-A-T isn't a dial you turn directly: "While E-E-A-T itself isn't a specific ranking factor, using a mix of factors that can identify content with good E-E-A-T is useful." Google's own self-assessment framing is **Who, How, and Why**:

- **Who** made it — add bylines and author information "where readers might expect it." Anonymous content on topics that need expertise reads as less trustworthy.
- **How** it was made — disclose automation or AI generation where that disclosure would help readers understand the content.
- **Why** it exists — "'Why' is perhaps the most important question." Content should be made "primarily to help people," not primarily to manipulate search rankings. This is the people-first test.

**YMYL** topics ("Your Money or Your Life" — health, finance, safety, legal, major life decisions) carry extra E-E-A-T weight, because low-quality content there can cause real harm.

Two myths worth killing:

- **There is no preferred word count.** In Google's words: "Are you writing to a particular word count because you've heard or read that Google has a preferred word count? (No, we don't.)" Write to the length the topic needs.
- **Date-churning doesn't create freshness.** Changing the published date or making cosmetic edits to seem "fresh" doesn't help. Update content when there's a real reason to.

Note on quality raters: their judgments don't feed ranking directly. "Rater data is not used directly in our ranking algorithms" — raters measure whether algorithm changes are working, they don't score your individual pages.

## Human-centric organization

Google's guide is explicit: write for humans first — "focus on human readability; don't worry about perfect code." The structural moves that help humans also help AI systems extract meaning:

- **Front-load the answer.** The first paragraph should state what the page is about and what conclusion the reader can expect. Don't bury the lede.
- **Clear, descriptive headings.** A reader skimming H2s should understand the shape of the article. Avoid clever titles for primary sections.
- **Real paragraphs, not bullet salad.** Bullets are great for lists of comparable items. Prose is better for reasoning, narrative, and tradeoffs. A page that's 90% bullets reads as low-effort.
- **One topic per section.** If a section talks about three things, split it.
- **Use tables for comparisons** (transit time by mode, weight class by service, etc.).
- **Show, then explain.** A photo of the actual process beats a stock image plus 200 words.

## Multimedia is a discovery surface

Generative AI features include images and video. Sites that publish high-quality, original media have more opportunities to appear. The rules are standard image/video SEO:

- **Real photos** of your operation, products, or people — not stock imagery. Stock images don't fail SEO outright, but they don't earn AI surfacing either.
- **Descriptive filenames** (`container-loading-izmir-port.jpg`, not `IMG_4438.jpg`).
- **Meaningful `alt` text** that describes what the image actually shows, in the page's primary language.
- **Captions** where the image carries information the body can't repeat.
- **Video transcripts and chapters** for any embedded video that contains real information.
- **`<picture>` / `next/image` with explicit dimensions** to avoid layout shift.
- **One canonical image URL per asset.** Don't serve the same hero image from five different paths.

## Avoid over-optimization

The fan-out mechanic (see [ai-search-mechanics.md](ai-search-mechanics.md)) tempts teams to create a separate page for every variation: "road freight to Hamburg," "road freight to Hamburg from Istanbul," "cheap road freight to Hamburg," and so on. Google's guide explicitly calls this out as scaled content abuse, which violates spam policy and can suppress the entire domain.

The healthier approach:

- **One strong page per real topic**, not per keyword variation.
- **Cover the natural sub-questions on that page.** If readers really do ask about transit times, weight classes, customs, and price ranges for road freight to Germany, address all of those on the same road-freight page.
- **Let semantic understanding do the matching.** Google understands that "lorry transport Germany," "TIR Almanya," and "road freight to Germany" mean similar things. You do not need three pages.

A useful heuristic: only spin up a new page if it has a substantively different answer, not just a different keyword.

## Using AI to generate content (the rules)

AI-generated content is **not banned**. Google's position ("using-gen-ai-content" + spam policies, updated 2025-12-10 / 2026-05-15) is that AI content must meet the same Search Essentials and spam policies as anything else. The line is value, not authorship:

> Using generative AI tools or other similar tools to generate many pages without adding value for users may violate Google's spam policy on scaled content abuse.

So the failure mode isn't "used AI" — it's mass-producing thin pages. Disclosure is encouraged where readers would expect it (see the **How** question under E-E-A-T).

Ecommerce carries concrete labeling requirements that are easy to miss:

- **AI-generated images** must carry IPTC `DigitalSourceType` metadata set to `TrainedAlgorithmicMedia`.
- **AI-generated product data** (title, description) "must be specified separately and labeled as AI-generated."

The takeaway: use AI as a tool behind genuinely helpful, people-first content — drafting, summarizing, restructuring real expertise — never to mass-produce thin pages at scale.

## Editing pass: catch commodity drift

When reviewing a draft, look for these tells of commodity content:

- The opening paragraph could appear verbatim on a competitor's site.
- "It is important to..." / "When choosing a logistics partner, there are several factors..." — these phrases are filler. Cut them.
- Every claim is generic: "fast," "reliable," "trusted." Replace with specifics: "average 3-day transit on Istanbul–Sofia," "less than 0.4% damage claim rate on industrial cargo in 2024."
- No numbers, no proper nouns, no dates.
- Stock photography or AI-generated illustrations as the only imagery.

A short, specific, opinionated page outperforms a long, vague, neutral one in both classical and AI search.
