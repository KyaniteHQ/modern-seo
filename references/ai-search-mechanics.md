# AI Search Mechanics: RAG and Query Fan-out

This is **Surface A** (see the two-surface model in [SKILL.md](../SKILL.md)): how Google's generative features *find and surface* your content. It is the ranking/retrieval surface, not the agent-usability surface. You don't optimize for these mechanics directly. You understand them so you can ignore bad advice and focus on the work that actually moves visibility.

## Retrieval-Augmented Generation (RAG)

When a user asks a question that triggers an AI Overview or an AI Mode response, the system does not generate from the model's parameters alone. It retrieves relevant indexed web pages — using Google's core ranking systems — and grounds the generated answer in those pages. The response typically links back to the sources with clickable citations.

**What this means in practice:**

- **Indexing is mandatory.** A page that isn't indexed cannot be retrieved, so it cannot ground an AI answer.
- **Ranking matters.** Retrieval favors pages that the ranking system already considers relevant and high-quality for the query. Pages that don't rank don't get retrieved.
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

## Practical implications for content design

1. **Write the main page deeply.** If you only have time for one thing, make the canonical page on your topic comprehensive and specific. Include the sub-questions readers actually ask.
2. **Make sub-topics findable inside the page.** Use real H2/H3 headings, anchor links, and a table of contents on long pages so that segments are easy to extract.
3. **Don't fragment the topic.** Resist the urge to spin off "long-tail" variants unless each has genuinely different content.
4. **Refresh, don't proliferate.** When you have new information on an existing topic, update the canonical page. Don't write a sequel post that competes with itself.
5. **Cite primary sources.** Linking out to authoritative sources strengthens grounding signals and is not penalized.

## A short mental model

AI Overviews ≈ "the snippet box, but bigger, with multiple sources stitched together."

Everything you'd do to win a featured snippet — answer the question clearly near the top, use precise language, structure with headings — works for AI Overviews too. The difference is mostly scale (more sources synthesized) and surface area (the answer is paraphrased rather than quoted). The optimization recipe doesn't change.
