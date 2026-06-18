# AI Search Mechanics: RAG and Query Fan-out

This is **Surface A** (see the two-surface model in [SKILL.md](../SKILL.md)): how Google's generative features *find and surface* your content. It is the ranking/retrieval surface, not the agent-usability surface. You don't optimize for these mechanics directly — academic evidence confirms this: C-SEO Bench (peer-reviewed at NeurIPS 2025, arXiv:2506.11097) and the SAGEO Arena preprint (arXiv:2602.12187) independently find that generation-stage content rewrites are largely ineffective or harmful, and that traditional retrieval ranking dominates. You understand these mechanics so you can ignore bad advice and focus on the work that actually moves visibility.

## Retrieval-Augmented Generation (RAG)

When a user asks a question that triggers an AI Overview or an AI Mode response, the system does not generate from the model's parameters alone. It retrieves relevant indexed web pages — using Google's core ranking systems — and grounds the generated answer in those pages. The response typically links back to the sources with clickable citations.

**What this means in practice:**

- **Indexing is mandatory.** A page that isn't indexed cannot be retrieved, so it cannot ground an AI answer.
- **Ranking matters.** Retrieval favors pages that the ranking system already considers relevant and high-quality for the query. Pages that don't rank don't get retrieved.
- **Freshness is a retrieval factor.** Google defines RAG as improving "quality, accuracy, and freshness" of AI responses — recency is an explicit criterion named in the AI optimization guide (developers.google.com/search/docs/fundamentals/ai-optimization-guide). A page not updated recently may be deprioritized when the query has a temporal dimension. (See [content-strategy.md](content-strategy.md) for freshness guidance.)
- **The link is real.** AI Overviews drive clicks. Pages cited as sources receive traffic from these surfaces, similar to traditional snippets.
- **Grounding is what protects accuracy.** This is also why "non-commodity content" wins — the model can already produce generic statements; the value of grounding is in what only your page knows.

**What this does *not* mean:**

- You do not need a special markup file to declare "this page is RAG-friendly." (See [mythbusting.md](mythbusting.md) on `llms.txt`.)
- You do not need to chunk your content into tiny pieces for retrieval — the system does its own segmentation.
- You do not need to write in a special "AI-readable" style. Clear writing for humans is sufficient.

## Query fan-out

For a sufficiently complex query, the AI system issues additional related queries internally — a fan-out — to gather a broader evidence base before composing the answer. Google's own example: a query about "fixing a lawn with weeds" might fan out to "best herbicides" and "chemical-free weed removal."

**What this means in practice:**

- **Coverage depth matters more than keyword precision.** A page that thoroughly addresses a topic — including the natural sub-questions a reader (or fan-out query) would ask — has more opportunities to be retrieved across the fan-out set.
- **One strong page > many thin pages.** A comprehensive page on "road freight from Turkey to Germany" that covers transit times, weight classes, customs, fuel surcharges, and route alternatives is more likely to be retrieved across multiple fan-out queries than five shallow pages each targeting one of those facets.
- **Internal linking helps.** When the fan-out lands on a related page on your domain, internal links from there back to your main page reinforce topical authority.

**What this does *not* mean:**

- It does **not** mean "make a page for every fan-out query." That is the trap. Creating "road freight Hamburg," "road freight Hamburg cheap," "road freight Hamburg fast" etc. as separate thin pages is scaled content abuse and violates Google's spam policy. The page that gets retrieved on "road freight Hamburg fast" is the page that already ranks for "road freight Hamburg" and happens to discuss transit times credibly.

## Non-Google AI surfaces — same content work, different indexation prerequisites

The unified model ([SKILL.md](../SKILL.md)) covers Surface A (Google ranking + AI Overviews/AI Mode) and Surface B (agents that browse your site). Non-Google generative engines — ChatGPT Search, Perplexity, Microsoft Copilot — are a related but distinct retrieval context that shares the same prerequisite: the content quality and E-E-A-T work that earns Google rankings earns visibility on these surfaces too. Only the indexation prerequisites differ. You do not write differently per engine.

Two independent studies confirm this. C-SEO Bench (Puerto et al., arXiv:2506.11097, peer-reviewed NeurIPS 2025 Datasets & Benchmarks Track) found most conversational-SEO rewrites are "largely ineffective" and "frequently have a negative impact on ranking," with "traditional SEO strategies significantly more effective." SAGEO Arena (Kim et al., arXiv:2602.12187, February 2026 preprint — not yet confirmed peer-reviewed) found GEO methods "remain largely impractical under realistic conditions and often degrade retrieval and reranking." Both point in the same direction: optimize for retrieval, not generation.

**Indexation prerequisites by engine:**

| Engine | Index | Indexation prerequisite |
|---|---|---|
| Google AI Overviews / AI Mode / Gemini | Google (Googlebot) | Covered by Surface A — see above |
| ChatGPT Search | Hybrid: historically Bing-backed (April 2025 OpenAI court testimony) + OpenAI's own growing OAI-SearchBot index | Allow OAI-SearchBot — the only officially-documented prerequisite (platform.openai.com/docs/bots). Bing indexation is a supporting, evolving factor, not a hard prerequisite; architecture is actively changing |
| Perplexity | Proprietary Perplexity index | Allow PerplexityBot ("we recommend allowing PerplexityBot," docs.perplexity.ai). Caveat: Cloudflare (2025-08-04) documented Perplexity using stealth crawlers with rotating user-agents that bypass robots.txt — robots.txt alone is not a guarantee |
| Microsoft Copilot | Bing (Bingbot) | Bing Webmaster Tools; Bingbot access |

**OAI-SearchBot and GPTBot are independent controls.** GPTBot is training-only. "A webmaster can allow OAI-SearchBot in order to appear in search results while disallowing GPTBot to indicate that crawled content should not be used for training" (platform.openai.com/docs/bots). Blocking training does not affect ChatGPT Search visibility.

**Directional signal on retrieval system divergence (single-vendor, not a planning target):** Ahrefs data shows the AIO-vs-organic top-10 overlap declined from roughly 76% (mid-2025) to roughly 38% (early 2026) as query fan-out expanded, with standalone AI assistants overlapping even less with Google's top-10. This is directional evidence that these are distinct retrieval systems — which explains why the indexation prerequisites differ. It is not a reason to produce per-engine content variants; both C-SEO Bench and SAGEO Arena show that approach is ineffective.

## Practical implications for content design

1. **Write the main page deeply.** If you only have time for one thing, make the canonical page on your topic comprehensive and specific. Include the sub-questions readers actually ask.
2. **Make sub-topics findable inside the page.** Use real H2/H3 headings, anchor links, and a table of contents on long pages so that segments are easy to extract.
3. **Don't fragment the topic.** Resist the urge to spin off "long-tail" variants unless each has genuinely different content.
4. **Refresh, don't proliferate.** When you have new information on an existing topic, update the canonical page. Don't write a sequel post that competes with itself.
5. **Cite primary sources.** Linking out to authoritative sources strengthens grounding signals and is not penalized.

## Measuring AI visibility

### Google Search Console

Google launched two reports on 2026-06-03 (UK first):

- **Search "Generative AI" performance report** — covers AI Overviews and AI Mode, blended (not separated). Impressions-focused.
- **Discover generative-AI report** — separate from the Search report.

**Impression baseline caveat:** an impression over-counting bug (logging error window approximately 2025-05-13 to 2026-04-27, disclosed 2026-04-03) means year-over-year impression comparisons within that window are unreliable.

**AI-features opt-out control:** removes content from Google's generative AI features without affecting standard organic ranking. Google's documentation states the opt-out "will not be used as a ranking signal for search results outside of these generative AI Search features" — a neutral strategic choice, not a penalty.

### Third-party tools — emerging category, no standard methodology

AI citation and visibility measurement is an **emerging category with no standard methodology**. Profound, Ahrefs Brand Radar, and Semrush AI Visibility Toolkit are starting points, but their methodologies differ, panels are proprietary, and their numbers should not be used as planning targets.

One important concept from this space: **ghost citations** (term coined by Kevin Indig, Growth Memo, April 2026; quantified jointly with Semrush, June 2026). A ghost citation occurs when a URL is cited as a source in an AI answer while the brand name never appears in the answer text. Being cited ≠ being recommended. Track brand-name mentions alongside citation counts.

## A short mental model

AI Overviews ≈ "the snippet box, but bigger, with multiple sources stitched together."

Everything you'd do to win a featured snippet — answer the question clearly near the top, use precise language, structure with headings — works for AI Overviews too. The difference is mostly scale (more sources synthesized) and surface area (the answer is paraphrased rather than quoted). The optimization recipe doesn't change.
