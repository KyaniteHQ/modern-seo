# Deep Research Report — Improving the `modern-seo` Skill
**Date:** 2026-06-18 · **Scope:** competitive skill analysis (18 skills), GEO/AEO literature, primary-source freshness since 2026-05, adversarial verification of 42 claims.

---

## 0. How this was researched (so you can trust the conclusions)

- **2 multi-agent workflows + independent orchestrator checks:** ~72 sub-agents, ~3.6M tokens, ~790 tool calls.
- **Two-stage verification.** Workflow 1 adversarially verified 22 contested *stat* claims against multiple sources, hunting for citation cartels (SEO blogs reciting one unverified source). Workflow 2 verified 20 *new factual developments* (dates, versions, spec names, study attributions) against **primary** sources only.
- **I personally cross-checked** the highest-stakes items against primary sources: Google's live AI optimization guide, the FAQ deprecation notice, Google's Search Central changelog, the WebMCP doc, and both academic arXiv papers.
- **Confidence is marked throughout.** "Primary" = Google/Chrome/W3C/arXiv/official spec/company docs. "Vendor" = SEO tool blogs (directional at best). Every claim is tagged with which **Surface** it applies to: **A** = Google ranking/AI Overviews, **B** = AI agents browsing your site, **C** = non-Google generative engines (ChatGPT/Perplexity/etc.).

**Headline:** The skill is in strong shape and its core identity is *vindicated* — by Google's own May 2026 guide **and** by two peer-reviewed papers. It needs ~5 concrete accuracy fixes (some genuinely time-sensitive) and has ~4 real net-new gaps. Almost every competitor makes the exact mistakes this skill was built to push back on.

---

## 1. Bottom line — what the research VALIDATES (keep and strengthen these)

The most important finding is that the skill's contrarian positions are now the *better-supported* ones:

| Skill's position | Now backed by |
|---|---|
| Two-surface model; "AEO/GEO is still SEO" | **Google's own words**, May 15 2026 guide: *"optimizing for generative AI search is optimizing for the search experience, and thus still SEO."* + a new June 5 2026 Google page on evaluating third-party "AEO/GEO" advice. |
| llms.txt does nothing for Google ranking | **Google primary source**, June 15 2026 note: llms.txt *"won't negatively or positively impact your visibility or rankings in Google Search, as Google Search ignores them."* Upgrade from "Mueller dismissive" to formal docs. |
| Reject content "chunking" / "AI-style rewriting" | **Google guide** + **two peer-reviewed papers**: C-SEO Bench (NeurIPS 2025, arXiv:2506.11097) — C-SEO rewrites *"largely ineffective… frequently have a negative impact on ranking; traditional SEO is significantly more effective."* SAGEO Arena (arXiv:2602.12187, Feb 2026) — GEO methods *"remain largely impractical… often degrade retrieval and reranking."* |
| Seeded/inauthentic mentions = spam | **Google spam policy**, updated May 15 2026, now explicitly names *"attempting to manipulate generative AI responses in Google Search."* |
| Structured data is **not** an AI-ranking requirement | **Ahrefs DiD study** (May 11 2026, 1,885 pages): adding JSON-LD produced **no positive** AI-citation uplift on any platform (AI Overviews arm was slightly *negative*, −4.6%). Schema's value is rich results + entity disambiguation, exactly as the skill says. |
| No separate AI index; don't make a page per fan-out query | **Google guide** confirms both verbatim. |
| **Don't cite vendor lift numbers as targets** | Vindicated hard — see §3. The 132%, 89%, 3.2×, "134–167 words", 40%-flat figures all collapsed under verification. The skill's restraint is its single biggest credibility asset. |

**Unique differentiator:** Surface B (WebMCP, UCP/AP2, Lighthouse agentic browsing, accessibility tree) appears in **none** of the 18 competing SEO skills in any serious form. This is the skill's moat — keep investing here.

---

## 2. The competitive landscape (18 skills profiled)

**Skills inspected:** coreyhaines31/marketingskills (ai-seo + 5 siblings, read locally), AgriciDaniel/claude-seo (25 sub-skills/18 agents, the most comprehensive), aaron-he-zhu/seo-geo-claude-skills (CORE-EEAT 80-item + CITE 40-item), Bhanunamikaze/Agentic-SEO-Skill, zubair-trabzada/geo-seo-claude (~8k stars), nowork-studio/NotFair, ReScienceLab/opc-skills, Auriti-Labs/geo-optimizer-skill, rampstackco/claude-skills (103 skills), gbessoni/seobuild-onpage, mykpono/ultimate-seo-geo, agencia-conversion/agentic-seo-skills, fseixas/super-geo-agent-readiness, sanity-io/agent-toolkit, Thibaultbm/claude-seo-geo, metawhisp/amazing-seo-skill, 01clauding/claude-seo-skill, luka2chat/geo-skills.

### What the best of them do that `modern-seo` doesn't (the real gaps)
1. **Per-engine retrieval specificity.** Nearly all of them break out ChatGPT (Bing-fed + OAI-SearchBot), Perplexity (own index), Gemini/AIO, Copilot, Claude as distinct citation surfaces. `modern-seo` is deliberately Google-centric and gives no actionable guidance for non-Google engines.
2. **AI-crawler taxonomy.** The better ones (geo-seo-claude, geo-optimizer, luka2chat) list 13–14 AI bots and crucially distinguish **OAI-SearchBot (search) vs GPTBot (training)**. `modern-seo` names only Google-Extended + CloudVertexBot.
3. **AI-visibility measurement.** They name Profound, Ahrefs Brand Radar, Semrush AI Toolkit, Otterly, Peec, gego/llmopt. `modern-seo` has **zero** measurement guidance.
4. **Entity / knowledge-graph depth** and **live data integration** (DataForSEO, Ahrefs MCP, GSC).

### What almost all of them get WRONG (anti-patterns to keep avoiding)
- **They treat GEO/AEO as a separate discipline** — all 18 do, to varying degrees. This directly contradicts Google's documented position and is precisely what `modern-seo` exists to correct. Several (NotFair) even *cite* C-SEO Bench and then contradict its conclusion.
- **Proprietary 0–100 scores with invented weights** (GEO Score, CORE-EEAT, CITE, SEO Health Score, Player Score, Trust Stack). None independently calibrated.
- **Recycled vendor stats** — the "134–167 word optimal block" appears verbatim in claude-seo and amazing-seo-skill; the Princeton GEO percentages are mis-cited everywhere. These are citation-cartel memes (see §3).
- **llms.txt as a ranking/citation signal** — recommended by coreyhaines ai-seo, geo-optimizer (18/100 of its score), amazing-seo, luka2chat, and others. Contradicted by Google's June 15 2026 note.
- **Internal contradictions** — claude-seo describes FAQPage in two conflicting states; rampstack calls llms.txt both a signal and not-a-signal; 01clauding tells users to optimize "primarily for top-10" off a "92% from top-10" figure that is demonstrably outdated (see §3, claim 10).
- **Invented Google system names** — seobuild-onpage presents "Gecko Score"/"Jetstream" as real Google ranking signals (they aren't).

> **Net:** `modern-seo` should *selectively absorb the genuine gaps* (per-engine prerequisites, crawler taxonomy, measurement) **while keeping its anti-hype, no-vendor-numbers, two-surface identity** — which the competitors validate by getting it wrong.

---

## 3. The myth audit — verified verdicts on the circulating stats

These are the stats competitors cite as fact. Verdicts are from adversarial multi-source verification. **None should be added as planning targets.**

| # | Claim (as circulated) | Verdict | What's actually true |
|---|---|---|---|
| 1 | "Cite sources = +132% AI Overview visibility" | **REFUTED** | A single cherry-picked query row in the GEO paper's Table 4, silently re-attributed to "Google research." Not a summary stat, not about Google. |
| 2 | "Authoritative tone = +89%" | **REFUTED** | The GEO paper explicitly found **no significant improvement** from authoritative tone. The 89% has no traceable origin. |
| 3 | "Optimal citability block = 134–167 words" | **UNVERIFIABLE** | One UK vendor (Cited By AI), no methodology; falsely attributed to Princeton. Ahrefs (174K pages) found ~zero correlation (ρ≈0.04) between word count and citation. The range actually matches the length of AI *outputs*, not source content. |
| 4 | "GEO methods = +40% visibility" | **PARTIALLY TRUE, heavily caveated** | Real (KDD 2024) but it's a proxy metric (PAWC) in a GPT-3.5 simulation with forced per-sentence citations and a 5-source zero-sum pool. Best method (Quotation Addition) +41%; Cite Sources +28%; Authoritative only +10%. Not a production system, not Google. Directional only. |
| 5 | "Brand mentions correlate 3× more than backlinks" | **PARTIALLY TRUE** | Real Ahrefs data (Spearman ρ 0.664 vs 0.218) but **dividing Spearman ρ's is statistically invalid**, heavily confounded, and Semrush found backlink *quality* correlates ~0.65 too. Use the *direction*, never "3×". |
| 6 | "Reddit/UGC disproportionately cited" | **PARTIALLY TRUE** | True for AI Overviews (over-indexed) but largely *downstream of Reddit's organic rank* (86% AIO-organic domain overlap). Strongest on Perplexity; ChatGPT **reduced** Reddit citations Sept 2025; Gemini cites it ~0.1–5%; **0% via all APIs**. Varies hugely by query type (consumer vs B2B/YMYL). |
| 7 | "No major LLM consumes llms.txt" | **PARTIALLY TRUE** | Confirmed for **Google** (primary). OpenAI/Anthropic/Perplexity: no documented retrieval use (absence of evidence). **But** agent dev-tools Cursor & Windsurf *do* consume llms.txt by design — a Surface-B nuance worth adding. |
| 8 | "ACP & AP2 are leading protocols alongside UCP" | **PARTIALLY TRUE w/ corrections** | ACP = "Agentic Commerce Protocol," **Stripe + OpenAI** (NOT "OpenAI's", NOT Meta — see §6). AP2 is **not** a peer of UCP — it's the payment layer that plugs *into* UCP. |
| 9 | "AIO/AI Mode use the same ranking systems as Search" | **CONFIRMED** | Google primary source. Shared index + indexed/snippet-eligible floor. But "rooted in" ≠ organic rank maps to citation order (see #10). |
| 10 | "AI citations strongly overlap with classic top-10" | **PARTIALLY TRUE, declining** | Moderate at best (Spearman 0.347). AIO-organic top-10 overlap fell **76% (Jul 2025) → 38% (Mar 2026)** as query fan-out expanded. Standalone assistants ~12%. Do **not** tell users to "optimize for top-10 because AI follows it." |
| 11 | "AIO appears in ~45% of searches" | **PARTIALLY TRUE** | Broad unbiased samples: 16–30% (2025). 45%+ is volume-weighted or informational-only. |
| 12 | "AIO cuts clicks up to 58%" | **PARTIALLY TRUE** | Ahrefs vendor figure (position-1, desktop, informational only). The one RCT (CMU/ISB) found 38%; Pew found ~47%. Directional yes; 58% is the high end. |
| — | SE Ranking "40/35/25" split; ZipTie "55/14/12"; "8.4 citations"; "3.2× freshness" | **PARTIALLY TRUE / UNVERIFIABLE** | All single-vendor, several **misattributed** (the "ZipTie" study is actually Sellm.io; the "40/35/25" split was invented by a blog, not in SE Ranking's data). Larger studies (AirOps, ConvertMate) find domain authority has *near-zero/negative* correlation with ChatGPT citation. |

**Directional findings that *are* durable** (safe to reflect qualitatively, never as numbers): third-party sources dominate commercial-discovery citations (~80–90%); genuine freshness helps (esp. Perplexity); answer-first, self-contained, factually-dense structure helps extraction on non-Google engines; entity consistency (Wikidata/sameAs) aids disambiguation.

---

## 4. Recommended changes — prioritized

### 🔴 HIGH — accuracy fixes (some time-sensitive); do these first

**H1 — `structured-data.md`: FAQ rich results are deprecated.** *(Primary-source confirmed; the skill is currently wrong here.)*
- Google's live FAQPage doc: *"As of May 7, 2026, FAQ rich results are no longer appearing in Google Search"* (reports/test removed June 2026, API Aug 2026). This ended even the gov/health exception that survived since Aug 2023.
- Also flag **HowTo** rich results (deprecated Sept 2023).
- Reframe both from "earns rich results" → "valid schema, no SERP rich result as of mid-2026; harmless to keep, don't add *for* rich results." Add a short "schema deprecation tracker."

**H2 — `technical-foundations.md`: AI-crawler taxonomy (the single highest-value change).** *(Primary: platform.openai.com/docs/bots.)*
- Today the skill implies crawler decisions are training-vs-not. For non-Google engines they're **indexation**-vs-not. A site that blocks GPTBot to opt out of training, and unknowingly blocks **OAI-SearchBot**, removes itself from **ChatGPT search citations** entirely. Real, avoidable harm.
- Add a 3-tier table: **(1) Training-only** (GPTBot, Google-Extended, ClaudeBot/anthropic-ai, CCBot — blocking ≠ citation effect); **(2) Search-index** (OAI-SearchBot → ChatGPT, PerplexityBot → Perplexity's own index, Bingbot → ChatGPT/Copilot — blocking removes you from that engine's citations); **(3) User-triggered fetchers** (ChatGPT-User, Perplexity-User — robots.txt loosely applied). Note CDN/WAF (Cloudflare) can block bots independent of robots.txt, and Cloudflare (Aug 4 2025) documented Perplexity using stealth crawlers — so robots.txt isn't a guarantee.

**H3 — `agentic-web-standards.md`: WebMCP API rename (your code example is now deprecated).** *(Primary: developer.chrome.com/docs/ai/webmcp/imperative-api + W3C CG draft.)*
- `navigator.modelContext` is **deprecated in Chrome 150** → use **`document.modelContext`** (spec PR #184 merged May 27 2026). Update the imperative-API code block.
- Update status: origin trial **Chrome 149–156**, unflagged shipping **Chrome 157**. Note the **Declarative API spec section is still marked TODO** → HTML-annotation surface is the more volatile one.

**H4 — `ai-search-mechanics.md`: Non-Google engine prerequisites (within the unified model).** *(Primary: OpenAI court testimony/docs; Perplexity Search API launch Sept 25 2025.)*
- Frame it as: *same quality work is the prerequisite for all surfaces; only the indexation prerequisites differ.* ChatGPT Search retrieves from **Bing** → Bing Webmaster Tools + Bingbot allowed. Perplexity runs its **own index** → PerplexityBot allowed. Use the overlap contrast (Google AIO ~38% top-10 overlap vs standalone assistants ~12%) as the *illustration of why these are distinct prerequisites* — with a clear "single-vendor, directional" caveat. **Do not** add per-engine content-rewriting advice (refuted by C-SEO Bench).

**H5 — `ai-search-mechanics.md`: AI-visibility measurement (currently zero coverage).** *(Primary: Google Search Central blog, June 3 2026.)*
- **GSC Generative AI Performance Reports** launched June 3 2026 (UK first): impressions for AI Overviews + AI Mode **+ Discover**; no click attribution; AIO/AI Mode blended. Note the **impression over-counting bug** (logging error May 13 2025 – Apr 27 2026, disclosed Apr 3 2026) — year-over-year baselines in that window are corrupted.
- **AI-features opt-out toggle** (June 3 2026): removes a site from AI features *without* affecting organic ranking — a new strategic decision to mention neutrally.
- Name the tool category (Profound, Ahrefs Brand Radar, Semrush AI Toolkit) as "emerging, no standard methodology." Introduce the **"ghost citations"** concept (Semrush/Indig, June 9 2026: 61.7% of citations use a brand's URL as a source while never naming the brand in the answer) — being *cited* ≠ being *recommended*.

### 🟡 MEDIUM — genuine net-new coverage

**M1 — `agentic-web-standards.md` + `business-local.md`: full agentic-commerce landscape.** *(All primary-source verified — see §6 for exact corrections.)* Add **ACP** (Stripe + OpenAI, launched Sept 29 2025) as a **sibling full-stack protocol to UCP**; clarify **AP2** is the FIDO-governed (donated Apr 28 2026) payment-authorization **layer inside UCP**, not a competitor. Update UCP (launched Jan 11 2026 w/ Shopify, Etsy, Wayfair, Target, Walmart; Cart/Catalog/Identity-Linking in the Apr 8 2026 spec). Keep the "watch, don't hand-roll; keep a clean fallback checkout" stance.

**M2 — `agentic-web-standards.md`: Agentic Resource Discovery (ARD).** *(Primary: Google Developers Blog, June 17 2026.)* New `/.well-known/ai-catalog.json` spec (v0.9 Draft, Apache 2.0, github.com/ards-project/ard-spec) enumerating MCP servers, A2A agents, OpenAPI tools — backed by Google, Microsoft, Hugging Face, GoDaddy, Linux Foundation AI Catalog WG. Frame as **"watch the spec"** (it's 1 day old, v0.9, no adoption data) + a pre-emptive note: *publishing it has zero Google-ranking effect.*

**M3 — `mythbusting.md`: sharpen earned-vs-seeded mentions.** Keep "seeded = spam" (now explicit Google policy) but add that **earned editorial mentions, accurate Wikidata/Wikipedia, and genuine YouTube presence** are legitimate, high-ROI authority work (Ahrefs Dec 12 2025: YouTube mentions were the strongest single correlate of brand AI visibility, Spearman ρ≈0.737 — correlation, not causation). Frame under E-E-A-T authoritativeness, not as an "AI hack." Don't restate the "3×" ratio.

**M4 — `content-strategy.md`: freshness as a quality signal.** Distinguish cosmetic **date-churning** (already correctly debunked) from **genuine content maintenance** (updating when facts change) — a real, user-serving freshness signal, especially for Perplexity. Use `dateModified` honestly. **No cadence number** (the 30/60/90-day prescriptions are vendor assertions).

**M5 — `mythbusting.md` / `ai-search-mechanics.md`: cite the academic backing.** Add C-SEO Bench (arXiv:2506.11097, NeurIPS 2025) and SAGEO Arena (arXiv:2602.12187) as peer-reviewed confirmation. Upgrades the skill from "Google says so" to "Google says so *and* peer-reviewed research confirms it."

**M6 — `SKILL.md` + `agentic-experiences.md`: Lighthouse status.** "Experimental" → **default config in Lighthouse 13.3.0** (May 7 2026); inherited by PageSpeed Insights (~May 21). Clarify: missing llms.txt = **Not Applicable** (not a failure); a 5xx on it = failure.

### 🟢 LOW — short notes / forward-looking heads-ups
- **`technical-foundations.md`:** Bing Webmaster Tools + **IndexNow** (Bing/Yandex/Seznam/Naver) as the non-Google indexing equivalents — relevant for ChatGPT/Copilot visibility only.
- **`agentic-web-standards.md`:** **RSL 1.0** (Really Simple Licensing, Dec 10 2025) — machine-readable AI-training-permission + compensation standard (Reddit/Yahoo/Medium/Quora launch; Cloudflare/Akamai joined for v1.0). Distinct from robots.txt and llms.txt; no ranking effect.
- **`agentic-web-standards.md`:** **MCP 2026-07-28 RC** makes MCP **stateless** (removes the initialize handshake + Mcp-Session-Id) — heads-up for devs building MCP servers now.
- **Surface A/B notes:** Google **Preferred Sources** now covers AI Overviews + AI Mode (May 27 2026) — a publisher discovery path; Google **"information agents"** (I/O May 19 2026; Ultra rollout June 12) run standing background queries — being the answer to a standing query is architecturally distinct from ad-hoc citation.

---

## 5. Explicitly REJECTED ideas (do NOT add — competitors do, and they're wrong)

- **"134–167 word optimal block"** — unverifiable vendor meme; ~zero empirical correlation.
- **Any GEO % as a planning target** (40% flat, 132%, 89%) — refuted/mis-attributed.
- **Per-engine content differentiation** ("write differently for ChatGPT") — C-SEO Bench/SAGEO show it's ineffective and often harmful; a multi-actor prisoner's dilemma.
- **AEO/AEO/GEO as a separate discipline** — contradicts Google's primary source.
- **Proprietary 0–100 GEO/CORE-EEAT/CITE scores** — invented weights, no calibration. (A lightweight *non-scored* checklist is fine: answer-in-first-paragraph, primary source cited, author named, `dateModified` accurate, `Organization sameAs` current.)
- **Vendor `.well-known/ai.txt`, `/ai/summary.json`, `/ai/faq.json` endpoints** — invented by one vendor, zero adoption. (ARD's `ai-catalog.json` is the legitimate one.)
- **Voice-search as a separate track** — reduces to Bing SEO.
- **Readability-score gates (Flesch 60–70)** — Google confirmed not a ranking signal.
- **The "3×" brand-mentions ratio** — even with caveats; the math is invalid.
- **Trustpilot "+52pp" stat** — misattributed (it's the *minimal*-profile tier) and Trustpilot-funded.

---

## 6. Corrections caught during verification (so the report itself is accurate)

The verification pass caught errors that an unverified synthesis would have shipped:
- ❌ "ACP co-created by Meta" → **Founding Maintainers are Stripe + OpenAI only.** Meta appears only in a post-launch docs update.
- ❌ "OpenAI Instant Checkout was shut down (Mar 2026)" → **Repositioned, not discontinued** ("moving to Apps"); ACP *expanded* (Cart, Feed, Orders, Auth, MCP) through April 2026.
- ❌ "Seer/Trustpilot +52pp for actively-managed profiles" → the 52pp is the **minimal** tier (T1); it's **Trustpilot-commissioned** research. Do not cite as independent.
- ❌ Critic's "SAGEO arXiv ID is hallucinated" → **the paper is real** (ID resolves; Kim et al., Yonsei). The critic's heuristic was outdated.
- ✅ Outdated "76% top-10 overlap" corrected to **38% (Mar 2026)**.
- ✅ Ahrefs schema study: not merely "no uplift" — **slightly negative for AI Overviews** (−4.6%), on already-cited pages.

---

## 7. Suggested next steps

1. **Land the 🔴 HIGH fixes** (H1–H5) — H1 (FAQ) and H3 (WebMCP API) are factual errors in the current skill; H2 (crawler taxonomy) is the highest-value single addition.
2. **Then 🟡 MEDIUM** — these expand coverage into the competitors' legitimate territory without diluting the skill's identity.
3. **Bump version → 2.1.0, last-reviewed → 2026-06**, and add the new primary sources (OpenAI bots doc, FIDO AP2 release, ACP/UCP specs, ARD blog, GSC AI reports, C-SEO Bench, SAGEO Arena) to the Sources list.

Raw artifacts (profiles, verdicts, verification buckets) are in `.research/`.

---

## 8. Validation addendum (2026-06-18) — every recommendation re-checked against OFFICIAL docs

A third pass (16 verifiers + my own first-hand fetches) re-verified each recommended change against first-party documentation (Google/Bing/OpenAI/Chrome/FIDO/W3C/arXiv). Result: **1 refuted framing, 12 confirmed-with-precision-fixes, 2 green.** No recommendation collapsed; the guidance stands, with the wording fixes below.

### 🟢 GREEN — ship as written
- **H3 WebMCP** — confirmed verbatim (`developer.chrome.com/docs/ai/webmcp/imperative-api`: *"`navigator.modelContext` is deprecated in Chrome 150. Use `document.modelContext` instead."*; OT 149–156, ships 157 per `chromestatus.com/feature/5117755740913664`). If the skill ever links a chromestatus feature ID, use `5117755740913664`.
- **MCP stateless RC** — confirmed verbatim (`blog.modelcontextprotocol.io`).

### 🔴 RED — DO NOT ship as written (refuted by official source)
- **H2/H4 "Bing Webmaster Tools / allowing Bingbot is a prerequisite for ChatGPT Search visibility" → REFUTED.** OpenAI's own docs name **OAI-SearchBot** (not Bingbot) as the crawler controlling ChatGPT Search visibility. A site could block Bingbot entirely and still appear in ChatGPT Search if OAI-SearchBot is allowed. "ChatGPT retrieves from Bing" was true at April 2025 court testimony but is increasingly outdated (OAI-SearchBot crawl volume tripled since Aug 2025; OpenAI's index is growing). **Correct framing:** Copilot = Bing (keep). For ChatGPT, the **primary, officially-documented gate is "allow OAI-SearchBot"**; Bing indexation is a *supporting, evolving* factor, not a prerequisite. Flag the architecture as actively changing.

### 🟡 AMBER — confirmed, apply these precision fixes before shipping
- **H1 FAQ/HowTo:** drop "(all sites)" — FAQ was gov/health-only since **Sept 14 2023**; **May 7 2026** is the final complete removal. HowTo exact date = **Sept 14 2023** (its doc now redirects to the changelog).
- **H2 OpenAI bots:** use OpenAI's terms — "ChatGPT search **answers/results**" (not "citations"); add the caveat that an OAI-SearchBot-blocked site "can still appear as **navigational links**."
- **H4 Perplexity:** Perplexity's docs say **"we recommend allowing PerplexityBot"** (not "must"). Cloudflare stealth-crawler report confirmed (Aug 4 2025).
- **H5 GSC:** there are **two** reports launched 2026-06-03 — a **Search** report (AI Overviews + AI Mode) and a separate **Discover** report — not one unified view. Opt-out + over-count bug (window 2025-05-13 → 2026-04-27) confirmed.
- **H5 tools/ghost citations:** full name is **"Semrush AI Visibility Toolkit."** "Ghost citations" was **coined by Kevin Indig (Growth Memo, Apr 2026)**, quantified jointly with Semrush (Jun 2026). All vendor-tier (fine for naming tools, not for stats).
- **M1 ACP:** "full-stack, comparable to UCP" is editorial, not a sourced quote — present as characterization. Instant Checkout's **native surface was deprecated**; merchants now build ChatGPT apps. Stripe+OpenAI co-creation / not-Meta / Sept 29 2025 all confirmed.
- **M1 AP2/UCP:** say **"checkout-and-payment mandate/authorization layer"** (AP2 defines a **Checkout Mandate** *and* a **Payment Mandate**) — "payment-authorization" alone is incomplete. AP2-plugs-into-UCP + FIDO donation (Apr 28 2026) confirmed.
- **M2 ARD:** broaden contributors — the spec credits **11 orgs** (AWS, Cisco, Databricks, GitHub, GoDaddy, Google, Hugging Face, Microsoft, Nvidia, Salesforce, Snowflake); the Linux Foundation AI Catalog WG provides the underlying data model. Name/path/date/v0.9/Apache-2.0 confirmed.
- **M5 academic:** **split the labels** — C-SEO Bench = peer-reviewed (NeurIPS 2025 D&B, OpenReview `oTeixD3oZO`); **SAGEO Arena = a Feb 2026 preprint, not confirmed peer-reviewed.** Both findings characterized accurately.
- **M6 Lighthouse:** "~May 21 PSI" is an **estimate** (release notes say "within 2 weeks") — label it so. v13.3.0 (~May 7), default-config move, N/A-vs-fail all confirmed.
- **L IndexNow:** supporters = **Bing, Naver, Seznam.cz, Yandex, Yep** (+ an Amazon endpoint). Copilot visibility confirmed (Bing Webmaster Blog); the **ChatGPT** link is *indirect* (via Bing) — qualify it.
- **L RSL 1.0:** it **complements** robots.txt (adds licensing/compensation), not merely "distinct from" it. Dec 10 2025 + backers confirmed.
- **L Preferred Sources / info agents:** Google's wording is **"AI Pro & Ultra"** (not "Ultra/Pro"); Ultra rollout is live globally as of June 12 2026. Dates (May 27 / May 19 2026) confirmed.

**Net:** of the HIGH edits — H3 ships as-is; H1, H2, H5 ship with wording fixes; **H4 must be restructured** (lead with OAI-SearchBot, demote Bing to a supporting/evolving factor). Every MEDIUM/LOW item is shippable once its precision fix is applied; none was refuted.
