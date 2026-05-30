---
name: modern-seo
description: 'Comprehensive, primary-source SEO and agentic-web guidance grounded in Google''s official AI optimization guide and Chrome''s agentic-browsing documentation. Use this skill whenever the user asks about SEO strategy that spans traditional Search and generative AI features (AI Overviews, AI Mode), wonders whether "AEO" or "GEO" is a real separate discipline, asks whether to invest in tactics like llms.txt, content chunking, or rewriting for LLMs, wants to understand how AI search retrieves and surfaces content (RAG, query fan-out), needs a unified mental model that ties together indexability, content quality, structured data, business listings, and agentic experiences, asks how to make a site usable by AI agents (WebMCP, the Lighthouse agentic-browsing category, the Universal Commerce Protocol), or asks generally how to optimize a site for "Google plus AI." Also use when the user says things like "should I optimize differently for AI," "is AEO real," "modern SEO," "what changed about SEO," "Google AI optimization guide," "AI Overviews ranking," "agent-ready website," "WebMCP," "agentic checkout," or "is llms.txt worth it." This is the comprehensive lens; for narrower tasks defer to: technical audits -> seo-audit, AI citation tactics for specific LLMs -> ai-seo, JSON-LD implementation -> schema, page generation at scale -> programmatic-seo, IA/URL planning -> site-architecture.'
metadata:
  version: 2.0.0
  sources:
    - https://developers.google.com/search/docs/fundamentals/ai-optimization-guide
    - https://developer.chrome.com/docs/lighthouse/agentic-browsing/scoring
    - https://developer.chrome.com/docs/lighthouse/agentic-browsing/llms-txt
    - https://developer.chrome.com/docs/ai/webmcp
    - https://web.dev/articles/ai-agent-site-ux
    - https://ucp.dev/latest/
  last_reviewed: 2026-05
---

# Modern SEO

This skill captures Google's and Chrome's own published guidance on being visible and usable in the era of generative AI. It exists to keep advice grounded in primary sources, especially when someone has been sold "AI SEO hacks" that the platforms themselves say are unnecessary.

The most important upgrade over naive "AI SEO" thinking is recognizing that there are **two different surfaces**, with two different rulebooks. Conflating them is the root cause of almost every bad recommendation in this space.

## The two-surface model (read this first)

> **Surface A — Getting found in Google Search and its AI features.** AI Overviews and AI Mode are built on Google's core ranking and quality systems. Google states it plainly: *"From Google Search's perspective, optimizing for generative AI search is optimizing for the search experience, and thus still SEO."* There is no second index, no separate "AI ranking." The work that earns classical visibility is the same work that earns AI-feature visibility. **"AEO" and "GEO" are not separate disciplines here.**
>
> **Surface B — Being usable by AI agents that browse and act on your site.** A genuinely new surface: third-party agents (in Chrome, in assistants, in shopping flows) that read your DOM and accessibility tree, click your buttons, fill your forms, and complete transactions. This is **not** about ranking. It has its own emerging standards (WebMCP, the Universal Commerce Protocol) and its own measurement (Chrome's Lighthouse "agentic browsing" category). Here, some things that are useless for Surface A — notably `llms.txt` — have a legitimate, optional role.

Keeping these straight resolves the contradictions people trip over:

| Tactic | Surface A (Google ranking) | Surface B (agent usability) |
|---|---|---|
| `llms.txt` | **Useless.** Google does not consume it. | **Optional.** Lighthouse checks for it as a discoverability hint for agents. |
| Content "chunking" | Unnecessary / can be spam | Irrelevant |
| Semantic HTML / clean a11y tree | Helps clarity | **Core requirement** |
| WebMCP tool annotations | Irrelevant to ranking | **The primary new lever** |
| Structured data | Optional (rich results) | Helps, not required |
| Non-commodity content | **The biggest lever** | Irrelevant to mechanics |

When a recommendation crosses a wire — e.g. "publish llms.txt to rank in AI Overviews" — it is conflating the two surfaces. Name the surface, then give the surface-correct answer.

## When to lean on this skill

- A broad SEO strategy question that doesn't fit cleanly into one narrower skill (`seo-audit`, `ai-seo`, `schema`, etc.).
- The user has been told to do something gimmicky (publish `llms.txt` *to rank*, "chunk" content, rewrite pages in a special "AI style") and you need to push back with authority.
- The user is mixing up retrieval mechanics (RAG, query fan-out) with content strategy, or mixing up Surface A and Surface B.
- The user wants their site to be operable by AI agents (WebMCP, agentic checkout, Lighthouse agentic-browsing score).
- The user is planning a new site or major rebuild and wants a foundation that holds up across classical search, AI search, and agentic use.

When the question is narrow and a specialized skill matches better, hand it off. Don't dump every principle on every question.

## Core principles

Route advice through these, in roughly this priority order.

### 1. SEO excellence is the prerequisite for AI visibility (Surface A)

Generative AI features use the same index, crawlers, and ranking systems as classical Search. A page that is not indexed and snippet-eligible cannot appear in AI Overviews — full stop. Before recommending anything exotic, verify the basics: crawlable, indexable, fast, free of duplicates, clear about its main content. Note Google's own caveat: meeting every requirement still does **not guarantee** crawling, indexing, or serving.

### 2. Non-commodity content wins (Surface A)

The single biggest lever for visibility is content that **cannot be easily replicated by an AI model**. Google contrasts:

- **Commodity content**: generic, recyclable, found in countless other articles ("7 Tips for First-Time Homebuyers").
- **Non-commodity content**: first-hand experience, specific operational detail, a take only this author could write ("Why We Waived the Inspection & Saved Money").

The test: *if you removed the author, would anything remain that isn't already on the open web?* If no, it's commodity content and loses in both classical and AI search. This connects to E-E-A-T, where Google says **trust is the most important component**. See [references/content-strategy.md](references/content-strategy.md).

### 3. Reject Surface-A gimmicks

Google's guide explicitly lists tactics you can **ignore for Google Search**:

- **`llms.txt` / AI text files / special markup** — *"You don't need to create new machine readable files, AI text files, markup, or Markdown to appear in generative AI search."* (This is about ranking — see Surface B for its real, separate role.)
- **Content "chunking"** — *"There's no requirement to break your content into tiny pieces... There's no ideal page length."*
- **Rewriting in a special "AI style"** — *"AI systems can understand synonyms and general meanings."* No special vocabulary, no long-tail keyword obsession.
- **Inauthentic "mentions"** — seeding fake mentions is filtered by spam systems and *"isn't as helpful as it might seem."*

If a user asks about any of these *as a ranking tactic*, say clearly that Google does not require them, then redirect. See [references/mythbusting.md](references/mythbusting.md).

### 4. AI retrieval mechanics shape what gets surfaced (Surface A)

Understand these to ignore bad advice, not to optimize for them directly:

- **Retrieval-Augmented Generation (RAG)**: AI Overviews retrieve relevant indexed pages and ground generated text in them, with clickable links back. Pages that already rank are eligible to be retrieved; pages that don't rank aren't.
- **Query fan-out**: a complex query spawns additional related queries internally. A page that thoroughly covers a topic (including its natural sub-questions) has more retrieval opportunities than a thin keyword-targeted page.

**Corollary**: do **not** create separate pages for every fan-out variation. That is scaled content abuse and violates spam policy. Cover the topic deeply on one strong page. See [references/ai-search-mechanics.md](references/ai-search-mechanics.md).

### 5. Technical foundations are non-negotiable (Surface A)

Google's three minimum technical requirements: **Googlebot isn't blocked**, the page returns **HTTP 200**, and the page has **indexable content**. Beyond the minimum:

- Indexed and snippet-eligible (no `noindex`, no `data-nosnippet` on main content).
- Crawlable (robots.txt allows Googlebot to fetch CSS/JS; reasonable crawl budget for large sites).
- Renders content Googlebot can read. JavaScript runs in a separate render pass — crawl, then render, then index — so server-render or pre-render critical content. Use real `<a href>` links and the History API, not fragment routing.
- Semantic HTML so the main content region is unambiguous (Google: *"focus on human readability; don't worry about perfect code"*).
- Fast, mobile-first, no intrusive interstitials, minimal duplication.
- To remove a page, use `noindex` (and keep it crawlable) — blocking in robots.txt won't reliably deindex.

See [references/technical-foundations.md](references/technical-foundations.md), which also covers AI-crawler controls (`Google-Extended`, `Google-CloudVertexBot`) and Next.js specifics.

### 6. Structured data is a best practice, not an AI requirement (Surface A)

Google is explicit: structured data is **not required** for generative AI features and there's no special schema you must add. It remains valuable for rich-result eligibility, knowledge-graph alignment, and reducing ambiguity. Implement it where a defined schema matches your content; never let markup disagree with the visible page. For ecommerce, AI-generated product data and images carry **labeling requirements** (IPTC `DigitalSourceType`). See [references/structured-data.md](references/structured-data.md).

### 7. Business listings carry product and local visibility (Surface A)

For ecommerce and local businesses, AI responses surface details from **Google Merchant Center** (products), **Google Business Profile** (local), and **Business Agent** conversational surfaces. A great website without a Merchant Center feed loses to a mediocre competitor that has one. Always ask whether these are set up before optimizing the website itself. See [references/business-local.md](references/business-local.md).

### 8. Build for agents — the new, real, non-gimmick surface (Surface B)

AI agents browse and act on the web on users' behalf: comparison shopping, booking, form-filling, checkout. They read the DOM, the rendered pixels, and **the accessibility tree** (their primary data model). This is a genuinely new discipline — not a ranking hack — with concrete, emerging standards:

- **Agent-friendly UX**: real semantic elements, stable layout (low CLS — agents use coordinates/screenshots), clean accessibility tree (names, roles, valid hierarchy), `<label for>`, visible hit targets, no ghost overlays. Same work as a serious accessibility pass.
- **WebMCP** (proposed standard, origin trial in Chrome 149): annotate forms and register JS tools so agents know exactly how to operate page features, with JSON-Schema inputs to reduce hallucination.
- **Chrome Lighthouse "agentic browsing" category** (experimental): scores agent-readiness as a pass-ratio (not 0–100) across WebMCP integration, agent-centric accessibility, and stability/discoverability (including an optional `llms.txt` check).
- **`llms.txt`** lives **here**, not in ranking: an optional Markdown summary at the domain root that helps agents understand your site faster. N/A if absent; not a Google ranking signal.
- **Universal Commerce Protocol (UCP)** (Google + Shopify, powers agentic checkout in AI Mode and Gemini): emerging. Don't implement bespoke UCP code yet, but build flows a future agent could operate.

See [references/agentic-experiences.md](references/agentic-experiences.md) and [references/agentic-web-standards.md](references/agentic-web-standards.md).

## How to apply this skill in conversation

This is a **knowledge reference** skill, not a prescribed audit workflow. When it triggers:

- **First, locate the surface.** Is the user asking about being *found/ranked* (Surface A) or being *operated by an agent* (Surface B)? Many confused questions resolve the moment you separate these.
- **When the user asks about a Surface-A gimmick** (llms.txt *to rank*, AI-specific markdown, chunking, "AI rewriting"): explain plainly that Google does not require it, cite the official position, and redirect to what works. Don't hedge.
- **When the user asks about `llms.txt`**: split the answer. *For Google ranking:* useless. *For AI agents browsing your site:* an optional, low-cost discoverability hint that Chrome's Lighthouse checks for. Neither makes you rank.
- **When the user asks how to rank in AI search**: anchor in "AI ranking ≈ classical ranking," then focus the highest-leverage gap (usually content uniqueness or indexability).
- **When the user asks to be "agent-ready"**: route to Surface B — accessibility-grade semantics first, then WebMCP for key flows, then UCP only if they're in commerce and tracking the standard.
- **When the user is excited about a fan-out / programmatic SEO play**: be honest about scaled-content-abuse risk; steer to deeper coverage on one strong page.
- **When the user runs ecommerce or local**: surface Merchant Center / Business Profile early — often higher ROI than on-page work.

Read the reference file that matches the specific question. Don't dump every principle on every conversation.

## Relationship to other skills

This skill is the comprehensive lens. For narrower tasks, prefer the specialized skill:

| Task | Use |
|---|---|
| Auditing a specific site or page | `seo-audit` |
| Tactics for being cited by specific LLMs (ChatGPT, Perplexity, Claude, Gemini) | `ai-seo` |
| Implementing JSON-LD structured data | `schema` |
| Generating pages at scale | `programmatic-seo` |
| Planning information architecture / URL structure | `site-architecture` |

If a question genuinely spans multiple domains, use this skill to frame the strategy and call out which specialized skill applies to each subtask.

## References

- [references/technical-foundations.md](references/technical-foundations.md) — indexability, crawlability, the crawl/render/index pipeline, JS SEO, AI-crawler controls, semantic HTML, UX/performance, duplicate content
- [references/content-strategy.md](references/content-strategy.md) — non-commodity content, E-E-A-T (Who/How/Why), first-person expertise, multimedia, AI-generated content rules, avoiding over-optimization
- [references/ai-search-mechanics.md](references/ai-search-mechanics.md) — RAG, query fan-out, what they imply for content design
- [references/structured-data.md](references/structured-data.md) — when to use, what types, JSON-LD patterns, AI-content labeling, common mistakes
- [references/business-local.md](references/business-local.md) — Merchant Center, Business Profile, Business Agent, agentic checkout, local SEO foundations
- [references/mythbusting.md](references/mythbusting.md) — llms.txt (and its real Surface-B role), chunking, AI-rewriting, inauthentic mentions, and why they don't help ranking
- [references/agentic-experiences.md](references/agentic-experiences.md) — browser agents, the accessibility tree, Lighthouse agentic-browsing scoring, agent-readiness audit
- [references/agentic-web-standards.md](references/agentic-web-standards.md) — WebMCP (declarative + imperative APIs), the Universal Commerce Protocol (UCP), AP2 mandates, what to adopt now vs. watch
