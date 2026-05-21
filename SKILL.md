---
name: modern-seo
description: Comprehensive, primary-source SEO guidance grounded in Google's official AI optimization guide. Use this skill whenever the user asks about SEO strategy that spans both traditional Search and generative AI features (AI Overviews, AI Mode), wonders whether "AEO" or "GEO" is a real separate discipline, asks whether to invest in tactics like llms.txt, content chunking, or rewriting for LLMs, wants to understand how AI search retrieves and surfaces content (RAG, query fan-out), needs a unified mental model that ties together indexability, content quality, structured data, business listings, and agentic experiences, or asks generally how to optimize a site for "Google plus AI." Also use when the user says things like "should I optimize differently for AI," "is AEO real," "modern SEO," "what changed about SEO," "Google AI optimization guide," "AI Overviews ranking," or "is llms.txt worth it." This is the comprehensive lens — for narrower tasks defer to: technical audits → seo-audit, AI citation tactics for specific LLMs → ai-seo, JSON-LD implementation → schema, page generation at scale → programmatic-seo, IA/URL planning → site-architecture.
metadata:
  version: 1.0.0
  source: https://developers.google.com/search/docs/fundamentals/ai-optimization-guide
---

# Modern SEO

This skill captures Google's own published guidance on optimizing for Search in the era of generative AI features. The central thesis is short and load-bearing:

> **AEO and GEO are not separate disciplines from SEO.** AI Overviews and AI Mode are built on Google's core ranking and quality systems. The work that earns visibility in classical Search is the same work that earns visibility in AI search. There is no second index to optimize for.

Use this skill to ground SEO advice in primary-source guidance — especially when someone has been sold on "AI SEO hacks" that Google itself has stated are unnecessary, or when they need a single coherent model spanning content, technical health, structured data, business listings, and agentic experiences.

## When to lean on this skill

- The user asks a broad SEO strategy question that doesn't fit cleanly into one of the narrower skills (`seo-audit`, `ai-seo`, `schema`, etc.).
- The user has been told to do something gimmicky (publish `llms.txt`, "chunk" their content, rewrite pages in a special "AI style") and you need to push back with authority.
- The user is mixing up retrieval mechanics (RAG, query fan-out) with content strategy and needs the two to be untangled.
- The user is planning a new site or major rebuild and wants a foundation that will hold up across both classical and AI search surfaces.

When the question is narrow and a specialized skill matches better, hand it off — don't dump every principle on every question.

## Core principles

These are the load-bearing ideas. Route advice through them in roughly this priority order.

### 1. SEO excellence is the prerequisite for AI visibility

Generative AI features use the same index, the same crawlers, and the same ranking systems as classical Search. A page that is not indexed and snippet-eligible cannot appear in AI Overviews — full stop. Before recommending anything exotic, verify the basics: the page is crawlable, indexable, fast, free of duplicates, and clear about its main content. If that work isn't done, nothing downstream will save the page.

### 2. Non-commodity content wins

The single biggest lever for visibility is creating content that **cannot be easily replicated by an AI model**. Google explicitly distinguishes:

- **Commodity content**: generic, recyclable, found in countless other articles. ("7 Tips for First-Time Homebuyers.")
- **Non-commodity content**: first-hand experience, specific operational details, a take only this author could write. ("Why We Waived the Inspection & Saved Money.")

When reviewing a page, ask the simple test: *if you removed the author, would anything remain that isn't already on the open web?* If no, the page is commodity content and will lose in both classical and AI search. See [references/content-strategy.md](references/content-strategy.md).

### 3. Reject "AEO/GEO" gimmicks

Several popular tactics are explicitly **not required** by Google and waste time:

- **`llms.txt` files** — Not used by Google's AI search. Publishing one is not a path to AI Overviews.
- **Content "chunking"** — Breaking pages into tiny atoms is unnecessary. Google's systems handle multiple topics on one page and extract relevant segments themselves.
- **Rewriting in a special "AI style"** — AI systems understand synonyms and meaning. There is no special vocabulary or sentence structure to adopt.
- **Inauthentic mentions** — Buying or seeding fake mentions in blogs/forums is filtered by spam-blocking systems and does not improve AI visibility.

If a user comes in asking about any of these, tell them clearly that Google does not require them, then redirect to what actually moves the needle. See [references/mythbusting.md](references/mythbusting.md).

### 4. AI retrieval mechanics shape what gets surfaced

Two mechanics worth understanding (not optimizing-for, but understanding):

- **Retrieval-Augmented Generation (RAG)**: AI Overviews retrieve relevant indexed pages and ground generated text in them, with clickable links back to sources. Pages that already rank well are eligible to be retrieved; pages that don't rank well aren't.
- **Query fan-out**: For a complex query, the system issues additional related queries internally. A question about "fixing a lawn with weeds" might fan out to "best herbicides" and "chemical-free weed removal." This means a page that thoroughly covers a topic — including its natural sub-questions — has more retrieval opportunities than a thin page targeting one keyword.

**Important corollary**: do **not** create separate pages for every fan-out variation. That is scaled content abuse and violates spam policy. Cover the topic deeply on one strong page instead. See [references/ai-search-mechanics.md](references/ai-search-mechanics.md).

### 5. Technical foundations are non-negotiable

To be retrievable, a page must clear standard Search requirements:

- Indexed and eligible for a snippet
- Crawlable (no accidental robots.txt blocks; reasonable crawl budget for large sites)
- Renders content Googlebot can read (JavaScript SEO matters on SPAs — server-render or pre-render when possible)
- Uses semantic HTML so the main content region is unambiguous
- Loads quickly and works on mobile
- Avoids duplicate content (canonical URLs, consistent internal linking)

See [references/technical-foundations.md](references/technical-foundations.md) for the working checklist and notes specific to JavaScript frameworks like Next.js.

### 6. Structured data is a best practice, not an AI requirement

Google's guide is explicit: structured data is **not mandatory** for appearing in generative AI features. It remains valuable because it qualifies pages for rich results, helps with knowledge graph alignment, and reduces ambiguity for crawlers — but don't sell it to the user as "the AI Overviews unlock." Implement it where it has a defined schema that matches your content (Article, Product, LocalBusiness, FAQ, BreadcrumbList, Organization). Don't fabricate schema for content types Google doesn't reward, and never let the markup disagree with what's on the page. See [references/structured-data.md](references/structured-data.md).

### 7. Business listings carry product and local visibility

For ecommerce and local businesses, AI responses often surface products and business details from:

- **Google Merchant Center** feeds (products)
- **Google Business Profile** (local services, hours, photos, reviews)
- **Business Agent** conversational surfaces (where available)

A great-looking website without a Merchant Center feed loses to a mediocre competitor that has one. If a user runs an ecommerce or local-services business, always ask whether these are set up and current before optimizing the website itself. See [references/business-local.md](references/business-local.md).

### 8. Sites should be agent-friendly

A new and rising surface: AI agents that browse the web on behalf of users — booking, comparison shopping, transactions. These agents read the DOM, the rendered pixels, and the accessibility tree. Sites with clear semantic structure, stable selectors, accessible labels, and unambiguous main-content regions are easier for agents to operate. The emerging Universal Commerce Protocol (UCP) is worth tracking but does not yet require implementation. See [references/agentic-experiences.md](references/agentic-experiences.md).

## How to apply this skill in conversation

This is a **knowledge reference** skill, not a prescribed audit workflow. When it triggers:

- **When the user asks about a gimmick** (llms.txt, AI-specific markdown files, content chunking, "AI rewriting"): explain plainly that Google does not require it, cite the official position, and redirect to what does work. Don't hedge — the user is being misinformed.
- **When the user asks how to rank in AI search**: anchor the answer in "AI ranking ≈ classical ranking," then focus the conversation on the highest-leverage gap (usually content uniqueness or indexability).
- **When reviewing a page or site**: walk the principles in priority order — indexability first, content uniqueness second, structured data and business listings third, agent-friendliness as forward-looking.
- **When the user is excited about a fan-out / programmatic SEO play**: be honest about the scaled-content-abuse risk and steer toward deeper coverage on one strong page rather than many shallow ones.
- **When the user runs an ecommerce or local business**: surface Merchant Center / Business Profile early — these often have higher ROI than on-page work.

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

- [references/technical-foundations.md](references/technical-foundations.md) — indexability, crawlability, JS SEO, semantic HTML, UX/performance, duplicate content
- [references/content-strategy.md](references/content-strategy.md) — non-commodity content, first-person expertise, multimedia, avoiding over-optimization
- [references/ai-search-mechanics.md](references/ai-search-mechanics.md) — RAG, query fan-out, what they imply for content design
- [references/structured-data.md](references/structured-data.md) — when to use, what types, JSON-LD patterns, common mistakes
- [references/business-local.md](references/business-local.md) — Merchant Center, Business Profile, Business Agent, local SEO foundations
- [references/mythbusting.md](references/mythbusting.md) — llms.txt, chunking, AI-rewriting, inauthentic mentions, and why they don't work
- [references/agentic-experiences.md](references/agentic-experiences.md) — browser agents, DOM/accessibility expectations, UCP
