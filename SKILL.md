---
name: modern-seo
description: 'Primary-source SEO and agentic-web guidance grounded in Google''s AI optimization guide and Chrome''s agentic-browsing docs. Use for SEO strategy spanning traditional Search and generative AI features (AI Overviews, AI Mode); whether "AEO"/"GEO" is a separate discipline; whether tactics like llms.txt, content chunking, or rewriting for LLMs are worth it; how AI search retrieves content (RAG, query fan-out); visibility in non-Google engines (ChatGPT, Perplexity) and which AI crawlers to allow (OAI-SearchBot, GPTBot, PerplexityBot); how to measure AI visibility; or making a site usable by AI agents (WebMCP, Lighthouse agentic-browsing, agentic commerce via UCP/ACP). Also triggers on "should I optimize differently for AI," "is AEO real," "modern SEO," "AI Overviews ranking," "agent-ready website," "WebMCP," "agentic checkout," or "is llms.txt worth it." For narrower tasks defer to: seo-audit, ai-seo, schema, programmatic-seo, site-architecture.'
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
| `llms.txt` | **Useless.** Google does not consume it (confirmed in Google's docs). | **Optional.** A discoverability hint Lighthouse checks for; some agent dev-tools (Cursor, Windsurf) consume it. |
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
- The user wants visibility in non-Google AI engines (ChatGPT, Perplexity, Copilot), asks which AI crawlers to allow or block, or wants to measure AI-search visibility.

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

- **Retrieval-Augmented Generation (RAG)**: AI Overviews retrieve relevant indexed pages and ground generated text in them, with clickable links back. Pages that already rank are eligible to be retrieved; pages that don't rank aren't. Google frames RAG as improving the *"quality, accuracy, and freshness"* of answers — genuine recency is a real signal.
- **Query fan-out**: a complex query spawns additional related queries internally. A page that thoroughly covers a topic (including its natural sub-questions) has more retrieval opportunities than a thin keyword-targeted page.

**Corollary**: do **not** create separate pages for every fan-out variation. That is scaled content abuse and violates spam policy. Cover the topic deeply on one strong page. C-SEO Bench (peer-reviewed, NeurIPS 2025) and the SAGEO Arena preprint confirm that generation-stage content rewrites are largely ineffective — retrieval ranking (ordinary SEO) is what dominates. See [references/ai-search-mechanics.md](references/ai-search-mechanics.md), which now also covers non-Google engines (ChatGPT, Perplexity) and how to measure AI visibility.

### 5. Technical foundations are non-negotiable (Surface A)

Google's three minimum technical requirements: **Googlebot isn't blocked**, the page returns **HTTP 200**, and the page has **indexable content**. Beyond the minimum:

- Indexed and snippet-eligible (no `noindex`, no `data-nosnippet` on main content).
- Crawlable (robots.txt allows Googlebot to fetch CSS/JS; reasonable crawl budget for large sites).
- Renders content Googlebot can read. JavaScript runs in a separate render pass — crawl, then render, then index — so server-render or pre-render critical content. Use real `<a href>` links and the History API, not fragment routing.
- Semantic HTML so the main content region is unambiguous (Google: *"focus on human readability; don't worry about perfect code"*).
- Fast, mobile-first, no intrusive interstitials, minimal duplication.
- To remove a page, use `noindex` (and keep it crawlable) — blocking in robots.txt won't reliably deindex.

See [references/technical-foundations.md](references/technical-foundations.md), which also covers AI-crawler controls as a **three-tier taxonomy** — training-only crawlers (`GPTBot`, `Google-Extended`) whose blocking has no search effect, vs. search-index crawlers (`OAI-SearchBot` for ChatGPT, `PerplexityBot` for Perplexity, `Bingbot` for Copilot) whose blocking removes you from that engine's answers, vs. user-triggered fetchers — plus Bing Webmaster Tools / IndexNow and Next.js specifics.

### 6. Structured data is a best practice, not an AI requirement (Surface A)

Google is explicit: structured data is **not required** for generative AI features and there's no special schema you must add. It remains valuable for rich-result eligibility, knowledge-graph alignment, and reducing ambiguity. Implement it where a defined schema matches your content; never let markup disagree with the visible page. Two rich-result types are now **retired** — `FAQPage` (gov/health-only since Sept 14 2023, fully removed May 7 2026) and `HowTo` (Sept 14 2023): the markup is harmless to keep but no longer produces a SERP feature, so don't add it *for* rich results. For ecommerce, AI-generated product data and images carry **labeling requirements** (IPTC `DigitalSourceType`). See [references/structured-data.md](references/structured-data.md).

### 7. Business listings carry product and local visibility (Surface A)

For ecommerce and local businesses, AI responses surface details from **Google Merchant Center** (products), **Google Business Profile** (local), and **Business Agent** conversational surfaces. A great website without a Merchant Center feed loses to a mediocre competitor that has one. Always ask whether these are set up before optimizing the website itself. See [references/business-local.md](references/business-local.md).

### 8. Build for agents — the new, real, non-gimmick surface (Surface B)

AI agents browse and act on the web on users' behalf: comparison shopping, booking, form-filling, checkout. They read the DOM, the rendered pixels, and **the accessibility tree** (their primary data model). This is a genuinely new discipline — not a ranking hack — with concrete, emerging standards:

- **Agent-friendly UX**: real semantic elements, stable layout (low CLS — agents use coordinates/screenshots), clean accessibility tree (names, roles, valid hierarchy), `<label for>`, visible hit targets, no ghost overlays. Same work as a serious accessibility pass.
- **WebMCP** (proposed standard; Chrome origin trial 149–156, ships unflagged in Chrome 157): annotate forms and register JS tools (via `document.modelContext`) so agents know exactly how to operate page features, with JSON-Schema inputs to reduce hallucination.
- **Chrome Lighthouse "agentic browsing" category** (now in Lighthouse's default config as of 13.3.0): scores agent-readiness as a pass-ratio (not 0–100) across WebMCP integration, agent-centric accessibility, and stability/discoverability (including an optional `llms.txt` check — a missing file is N/A, not a failure).
- **`llms.txt`** lives **here**, not in ranking: an optional Markdown summary at the domain root that helps agents (and some dev-tools like Cursor/Windsurf) understand your site faster. N/A if absent; not a Google ranking signal.
- **Agentic commerce — UCP & ACP** (the two full-stack standards): **UCP** (Google + Shopify, launched Jan 2026) powers agentic checkout in AI Mode and Gemini; **ACP** (Stripe + OpenAI, Sept 2025) is the sibling protocol in the ChatGPT/Stripe ecosystem. **AP2** (donated to the FIDO Alliance, Apr 2026) is the payment-authorization layer that plugs *into* UCP — not a competitor. Watch, don't hand-roll; Shopify/Stripe merchants get coverage via their platform. Keep a clean web checkout as the fallback.
- **Agent discovery — ARD** (Agentic Resource Discovery, announced Jun 2026): an emerging `/.well-known/ai-catalog.json` spec enumerating MCP servers, A2A agents, and tools. Very new — watch the spec; it has no Google-ranking effect.

See [references/agentic-experiences.md](references/agentic-experiences.md) and [references/agentic-web-standards.md](references/agentic-web-standards.md).

### 9. Non-Google AI engines: same work, different indexation prerequisites

ChatGPT, Perplexity, and Copilot also cite the web, but from their **own** retrieval systems. This extends Surface A's logic; it is **not** a separate discipline and **not** a reason to write differently per engine (C-SEO Bench and SAGEO Arena both find per-engine content rewriting ineffective or harmful). What differs is the *indexation prerequisite*: allow **`OAI-SearchBot`** for ChatGPT search (OpenAI's officially documented lever — ChatGPT is a Bing-plus-own-index hybrid, so don't tell users to "optimize Bing to appear in ChatGPT"), allow **`PerplexityBot`** for Perplexity (its own index), and use Bing Webmaster Tools for Copilot. Same quality content, different doors. See [references/ai-search-mechanics.md](references/ai-search-mechanics.md).

### 10. Measure AI visibility — don't guess

There's now a feedback loop. Google Search Console ships **Generative AI performance reports** (impressions for AI Overviews + AI Mode; a separate one for Discover) and an **AI-features opt-out** that doesn't affect organic ranking. Third-party tools (Profound, Ahrefs Brand Radar, Semrush AI Visibility Toolkit) are an emerging category with no standard methodology. Watch for **"ghost citations"** — your URL used as a source while your brand name never appears in the answer (cited ≠ recommended). Treat every vendor lift statistic as directional, never a planning target. See [references/ai-search-mechanics.md](references/ai-search-mechanics.md).

## How to apply this skill in conversation

This is a **knowledge reference** skill, not a prescribed audit workflow. When it triggers:

- **First, locate the surface.** Is the user asking about being *found/ranked* (Surface A) or being *operated by an agent* (Surface B)? Many confused questions resolve the moment you separate these.
- **When the user asks about a Surface-A gimmick** (llms.txt *to rank*, AI-specific markdown, chunking, "AI rewriting"): explain plainly that Google does not require it, cite the official position, and redirect to what works. Don't hedge.
- **When the user asks about `llms.txt`**: split the answer. *For Google ranking:* useless. *For AI agents browsing your site:* an optional, low-cost discoverability hint that Chrome's Lighthouse checks for. Neither makes you rank.
- **When the user asks how to rank in AI search**: anchor in "AI ranking ≈ classical ranking," then focus the highest-leverage gap (usually content uniqueness or indexability).
- **When the user asks to be "agent-ready"**: route to Surface B — accessibility-grade semantics first, then WebMCP for key flows, then UCP only if they're in commerce and tracking the standard.
- **When the user is excited about a fan-out / programmatic SEO play**: be honest about scaled-content-abuse risk; steer to deeper coverage on one strong page.
- **When the user runs ecommerce or local**: surface Merchant Center / Business Profile early — often higher ROI than on-page work.
- **When the user wants ChatGPT / Perplexity visibility**: it's the same content work; the lever is letting their crawlers in — allow `OAI-SearchBot` (ChatGPT) and `PerplexityBot` (Perplexity), and don't conflate that with Bing. Resist per-engine content rewrites.
- **When the user asks how to track AI visibility**: point to GSC's Generative AI reports and the third-party tool category, and warn that vendor lift numbers are directional, not targets.

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

- [references/technical-foundations.md](references/technical-foundations.md) — indexability, crawlability, the crawl/render/index pipeline, JS SEO, the three-tier AI-crawler taxonomy (training vs. search-index vs. user-triggered), Bing Webmaster Tools / IndexNow, semantic HTML, UX/performance, duplicate content
- [references/content-strategy.md](references/content-strategy.md) — non-commodity content, E-E-A-T (Who/How/Why), first-person expertise, freshness as a quality signal, multimedia, AI-generated content rules, avoiding over-optimization
- [references/ai-search-mechanics.md](references/ai-search-mechanics.md) — RAG, query fan-out, non-Google engine prerequisites (ChatGPT, Perplexity), measuring AI visibility, what they imply for content design
- [references/structured-data.md](references/structured-data.md) — when to use, what types, FAQ/HowTo rich-result deprecation, JSON-LD patterns, AI-content labeling, common mistakes
- [references/business-local.md](references/business-local.md) — Merchant Center, Business Profile, Business Agent, agentic checkout (UCP / ACP / AP2), Preferred Sources, local SEO foundations
- [references/mythbusting.md](references/mythbusting.md) — llms.txt (and its real Surface-B role), chunking, AI-rewriting, inauthentic mentions, "GEO is a separate discipline," and why they don't help ranking (with peer-reviewed backing)
- [references/agentic-experiences.md](references/agentic-experiences.md) — browser agents, the accessibility tree, Lighthouse agentic-browsing scoring (now default config), agent-readiness audit
- [references/agentic-web-standards.md](references/agentic-web-standards.md) — WebMCP (`document.modelContext`), UCP and ACP, AP2 (FIDO), ARD (`/.well-known/ai-catalog.json`), RSL, MCP, what to adopt now vs. watch

## Sources and versioning

- **Skill version**: 2.1.2
- **Last reviewed**: 2026-06

**What changed in 2.1.2** (surfaced by a test-driven audit loop, each verified against primary sources): added form-field affordances (`autocomplete` tokens = WCAG 2.1 SC 1.3.5 + autofill; `type`/`inputmode`/`required`) to the Surface-B agent-readiness patterns; sitemap note that Google ignores `<priority>`/`<changefreq>` and uses an honest `<lastmod>`; `BreadcrumbList` fragment-URL caveat; `LocalBusiness`-requires-`address`-for-rich-results correction; machine-readable files must be discoverable.

**What changed in 2.1.1** (packaging only, no content change): added `"skills": ["./"]` to `.claude-plugin/plugin.json` so the plugin's root `SKILL.md` is exposed to the Claude Code plugin loader on all client versions (root-`SKILL.md` auto-discovery otherwise requires Claude Code v2.1.142+).

**What changed in 2.1.0** (all re-verified against official first-party docs): three-tier AI-crawler taxonomy (`OAI-SearchBot` vs `GPTBot`, `PerplexityBot`, `Bingbot`); non-Google engine prerequisites and AI-visibility measurement (GSC Generative AI reports, "ghost citations"); `FAQPage`/`HowTo` rich-result deprecation; WebMCP `document.modelContext` (Chrome 150) + origin-trial/ship versions; Lighthouse agentic-browsing now in default config; agentic-commerce landscape expanded (UCP + ACP, AP2→FIDO) plus ARD, RSL, and the MCP stateless RC; freshness as a quality signal; academic backing (C-SEO Bench, peer-reviewed; SAGEO Arena, preprint) for the anti-GEO-hype stance.

Grounded in these primary sources (verify against the live docs, which change):

- Google — AI optimization guide: https://developers.google.com/search/docs/fundamentals/ai-optimization-guide
- Google — Search Central documentation updates (changelog): https://developers.google.com/search/updates
- Google — FAQ structured data (deprecation notice): https://developers.google.com/search/docs/appearance/structured-data/faqpage
- Google — Search Console Generative AI performance reports: https://developers.google.com/search/blog
- OpenAI — Overview of OpenAI crawlers (GPTBot, OAI-SearchBot, ChatGPT-User): https://platform.openai.com/docs/bots
- Perplexity — bots / PerplexityBot: https://docs.perplexity.ai
- Microsoft — Copilot (Bing) and IndexNow: https://www.bing.com/indexnow
- Chrome — Lighthouse agentic browsing (scoring): https://developer.chrome.com/docs/lighthouse/agentic-browsing/scoring
- Chrome — Lighthouse agentic browsing (llms.txt): https://developer.chrome.com/docs/lighthouse/agentic-browsing/llms-txt
- Chrome — WebMCP (imperative API, `document.modelContext`): https://developer.chrome.com/docs/ai/webmcp
- web.dev — Agent-friendly website UX: https://web.dev/articles/ai-agent-site-ux
- Universal Commerce Protocol (UCP): https://ucp.dev/latest/
- Agentic Commerce Protocol (ACP): https://www.agenticcommerce.dev
- AP2 / Agent Payments Protocol (FIDO Alliance): https://ap2-protocol.org
- Agentic Resource Discovery (ARD): https://github.com/ards-project/ard-spec
- Really Simple Licensing (RSL): https://rslstandard.org
- Model Context Protocol (spec): https://modelcontextprotocol.io
- C-SEO Bench (NeurIPS 2025): https://arxiv.org/abs/2506.11097
- SAGEO Arena (preprint): https://arxiv.org/abs/2602.12187
