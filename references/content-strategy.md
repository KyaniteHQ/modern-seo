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

Google evaluates content quality through **E-E-A-T**: Experience, Expertise, Authoritativeness, Trustworthiness ("Creating helpful, reliable, people-first content", updated 2026-10-05). The hierarchy matters:

> Of these aspects, trust is most important. The others contribute to trust, but content doesn't necessarily have to demonstrate all of them.

E-E-A-T isn't a dial you turn directly: "While E-E-A-T itself isn't a specific ranking factor, using a mix of factors that can identify content with good E-E-A-T is useful." Google's own self-assessment framing is **Who, How, and Why**:

- **Who** made it — add bylines and author information "where readers might expect it." Anonymous content on topics that need expertise reads as less trustworthy. Never invent authors. Fabricated creator profiles, such as "AI-generated headshots, made-up names, or false credentials," are "a form of deception" in Google's words (see Myth 12 in [mythbusting.md](mythbusting.md)).
- **How** it was made — disclose automation or AI generation where that disclosure would help readers understand the content.
- **Why** it exists — "'Why' is perhaps the most important question." Content should be made "primarily to help people," not primarily to manipulate search rankings. This is the people-first test.

**YMYL** topics ("Your Money or Your Life" — health, finance, safety, legal, major life decisions) carry extra E-E-A-T weight, because low-quality content there can cause real harm.

Earned third-party authority — editorial mentions in relevant publications, accurate Wikipedia and Wikidata entries, a genuine YouTube presence — is legitimate authoritativeness work; for the earned-vs-seeded distinction see [mythbusting.md](mythbusting.md) Myths 5 and 7.

Two myths worth killing:

- **There is no preferred word count.** In Google's words: "Are you writing to a particular word count because you've heard or read that Google has a preferred word count? (No, we don't.)" Write to the length the topic needs.
- **Date-churning doesn't create freshness.** Changing the published date or making cosmetic edits to seem "fresh" doesn't help. Update content when there's a real reason to.

Note on quality raters: their judgments don't feed ranking directly. "Rater data is not used directly in our ranking algorithms" — raters measure whether algorithm changes are working, they don't score your individual pages.

**Main content and what raters judge** (same page, updated 2026-10-05). Main content is "any part of the web page that directly helps the page achieve its purpose," including tools, user contributions, tabbed sections, and headings. Raters judge it on **Effort**, **Originality** ("unique, original information or perspectives that aren't already available on other websites"), **Talent or skill**, and **Accuracy** (YMYL content must be "highly accurate and consistent with established expert consensus"). On effort: "using generative AI to produce large amounts of text without manual oversight or curation represents little to no effort," and "Attribution or giving credit to other sources doesn't replace the need for original effort."

## Freshness as a quality signal

The date-churning myth above covers the failure mode. The legitimate counterpart is **genuine content maintenance**: updating a page when the underlying facts change — prices, regulations, statistics, software versions, product availability. When the information on a page becomes inaccurate, correcting it serves users and is a real quality signal.

Google's RAG documentation explicitly aims to improve "the quality, accuracy, and **freshness**" of AI responses (AI optimization guide, developers.google.com/search/docs/fundamentals/ai-optimization-guide, updated 2026-07-10). Recency carries additional weight on AI search surfaces beyond Google: Perplexity, which runs its own proprietary index, particularly weights fresh content for real-time queries.

Express freshness honestly:

- Use `Article` or `BlogPosting` schema with a genuine `dateModified` value — set it to the date of a substantive update, not a cosmetic one.
- Keep the visible published and updated dates consistent with the schema markup.
- Do not roll `dateModified` forward without a corresponding change to the content.

There is no correct update cadence. 30/60/90-day schedules are vendor-sourced assertions with no basis in Google's published documentation. The governing principle: **update when accuracy changes, not on a schedule.**

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

**Recent enforcement dates** (Search Status Dashboard): spam updates ran 2026-06-24 to 06-26, 08-18 to 08-21, and 09-24 to 10-08; the last core update was May 2026 (05-21 to 06-02). Google gave no targeting details beyond "applies globally and to all languages," so don't infer a target.

## Using AI to generate content (the rules)

AI-generated content is **not banned**. Google's position ("using-gen-ai-content" + spam policies, updated 2026-10-01 / 2026-08-28) is that AI content must meet the same Search Essentials and spam policies as anything else. The line is value, not authorship:

> …using generative AI tools or other similar tools to generate many pages without adding value for users may violate Google's spam policy on scaled content abuse.

So the failure mode isn't "used AI" — it's mass-producing thin pages. Disclosure is encouraged where readers would expect it (see the **How** question under E-E-A-T).

Accuracy is the second failure mode: "generative models don't retrieve facts, but predict a likely sequence of words." Google says "It is critical to manually factcheck and review all AI-generated content for accuracy and trustworthiness before publishing," including `<title>` elements, meta descriptions, structured data, and image alt text.

Ecommerce carries concrete labeling requirements that are easy to miss:

- **AI-generated images** must carry IPTC `DigitalSourceType` metadata set to `TrainedAlgorithmicMedia`.
- **AI-generated product data** (title, description) "must be specified separately and labeled as AI-generated."

The takeaway: use AI as a tool behind genuinely helpful, people-first content — drafting, summarizing, restructuring real expertise — never to mass-produce thin pages at scale.

## Site reputation policy

Google's former "site reputation abuse" policy is now the **site reputation policy** (spam policies, updated 2026-08-28). It covers third-party content published on a host "mainly because of that host's already-established ranking signals." Third-party content alone is fine; wire services, syndicated news, forums, reader-facing advertorials, and appropriately treated affiliate links are not inconsistent with it.

A human review asks whether the section is "fully integrated" with the host. Google lists four factors, none decisive alone:

- Presentation consistent with the host's design and UX.
- Quality in line with the main domain.
- Stated or implied authorship and editorial responsibility.
- Identical or near-identical copies on other sites.

Google's examples: a curated, disclosed coupons section built with a partner (action unlikely); an unauthored CBD affiliate article copied from a marketplace (action likely outside the EEA); a freelancer's bylined cooking section under editorial oversight (action unlikely).

Enforcement depends on **where the searcher is**. The review "applies globally," but manual actions affect only results shown outside the European Economic Area (EEA). Inside the EEA, the section may be ranked separately on its own merits, and that categorization "isn't used as a ranking signal." Past EEA manual actions are lifted. Eligible sites can take disputes to mediation after a reconsideration request. Google's blog dated the EEA change from 2026-08-30.

## Editing pass: catch commodity drift

When reviewing a draft, look for these tells of commodity content:

- The opening paragraph could appear verbatim on a competitor's site.
- "It is important to..." / "When choosing a logistics partner, there are several factors..." — these phrases are filler. Cut them.
- Every claim is generic: "fast," "reliable," "trusted." Replace with specifics: "average 3-day transit on Istanbul–Sofia," "less than 0.4% damage claim rate on industrial cargo in 2024."
- No numbers, no proper nouns, no dates.
- Stock photography or AI-generated illustrations as the only imagery.

A short, specific, opinionated page outperforms a long, vague, neutral one in both classical and AI search.
