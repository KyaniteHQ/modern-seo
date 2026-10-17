# Agentic Web Standards: WebMCP, UCP/ACP, AP2, ARD

This is **Surface B** (see [the two-surface model in SKILL.md](../SKILL.md)): being *usable by AI agents that act on your site*, as opposed to being *found by Google's ranking systems*. None of this affects Google Search ranking. It is a separate, emerging discipline with its own standards.

The standards below are **proposed/experimental** as of this skill's last review (2026-06). The guidance is: understand them, adopt the low-risk progressive-enhancement pieces for flows you care about, and don't build brittle bespoke integrations against specs that are still changing.

## WebMCP — exposing your page's tools to agents

> "WebMCP is a proposed web standard to help you build and expose structured tools for AI agents. WebMCP provides JavaScript and annotates HTML form elements so that agents know exactly how to interact with page features, to support a user's experience."

The core idea: instead of an agent guessing how your UI works from the DOM, **your site declares its capabilities**. Tools execute on your page **visibly** (the user sees what happens), and WebMCP is a **progressive enhancement** — it layers on top of working HTML/JS, it doesn't replace it.

WebMCP supports three things:

- **Discovery** — register named tools (`checkout`, `filter_results`) that an agent can find.
- **JSON Schemas** — explicit input/output definitions to reduce hallucination or misunderstanding.
- **State** — a shared understanding of current page context.

**Status:** Developed openly (`webmachinelearning/webmcp`), under active discussion and subject to change. Chrome's origin trial runs **Chrome 149–162**; it was extended from 156 through an Intent to Extend Experiment approved on 2026-09-28 (chromestatus.com/feature/5117755740913664). **No ship milestone is set and no Intent to Ship has been filed.** On 2026-09-29 the Chrome team wrote that it is "hoping to move to I2S [Intent to Ship] within the extension window" (blink-dev, "Intent to Extend Experiment: WebMCP"). Chrome 162 reaches Stable on 2027-02-02; Chrome now ships a Stable milestone every two weeks from Chrome 153, so read milestone numbers against chromiumdash.appspot.com rather than counting four-week cycles. Testing flag `chrome://flags/#enable-webmcp-testing`. Gated by the `tools` Permissions Policy: it defaults to `self`, cross-origin iframes need `allow="tools"`, and a `Permissions-Policy: tools=()` header turns WebMCP off for a page and its frames (developer.chrome.com/docs/ai/webmcp, updated 2026-10-07).

**Limits Chrome states** (developer.chrome.com/docs/ai/webmcp, updated 2026-10-07): the API is "primarily designed for local browser workflows with a human in the loop" (headless use "may be possible"), and "Clients and browsers must visit a site directly to know if it has callable tools." WebMCP does not advertise your tools to an agent that has not loaded the page.

### Declarative API (annotate HTML forms) — Chrome trial only

The quickest way to try WebMCP in Chrome's trial, but **no longer part of the spec draft**: spec PR #338 (merged 2026-10-08) removed the form attributes; declarative WebMCP "is still being explored" in a separate explainer (github.com/webmachinelearning/webmcp/pull/338). Chrome still documents and ships it in the trial (developer.chrome.com/docs/ai/webmcp/declarative-api, updated 2026-09-25), and Lighthouse still audits it. Chrome's team reported that "the imperative API is a lot more popular than the declarative one" (blink-dev, 2026-09-29). Annotate an existing `<form>`:

```html
<form
  toolname="newsletter_signup"
  tooldescription="Subscribes the user to the weekly newsletter">
  <input
    name="email"
    type="email"
    toolparamdescription="The user's email address">
  <button type="submit">Sign Up</button>
</form>
```

**Volatility note:** Treat these attributes as a Chrome-trial convenience that is less certain to survive standardization than `registerTool()`. Use them as progressive enhancement only, never as a production dependency. From Chrome 156, `toolactivated` and `toolcancel` fire on `document.modelContext`; `window.ontoolactivated` and `window.ontoolcancel` are no longer supported (developer.chrome.com/docs/ai/webmcp/declarative-api, updated 2026-09-25).

Rules that the Lighthouse audit enforces (see scoring in [agentic-experiences.md](agentic-experiences.md)):

- `toolname` and `tooldescription` go **together** on the `<form>`. Having one without the other is a schema-validity failure.
- Every input the tool uses needs a unique `name`.
- Add `toolparamdescription` (and an associated `<label>`) to each input; missing these triggers warnings.
- Use action-oriented names (`book_appointment`, not `form1`) and describe **what the tool does and when to use it**.

### Imperative API (register JS tools)

For interactions that aren't a simple form submit:

**Use `document.modelContext`.** `navigator.modelContext` was deprecated in Chrome 150 and removed in Chrome 152 (github.com/GoogleChrome/lighthouse/pull/17129, merged 2026-07-16). Chrome's Imperative API page documents only `document.modelContext` (developer.chrome.com/docs/ai/webmcp/imperative-api, updated 2026-10-16).

```js
document.modelContext.registerTool({
  name: 'toggle_layer',
  description:
    'Control pizza layers (sauce, cheese). Use "add", "remove", or "toggle".',
  inputSchema: {
    type: 'object',
    properties: {
      layer: { type: 'string', enum: ['sauce-layer', 'cheese-layer'] },
      action: { type: 'string', enum: ['add', 'remove', 'toggle'] },
    },
    required: ['layer'],
  },
  execute: async ({ layer, action }) => {
    await toggleLayer(layer, action);
    return `Performed ${action || 'toggle'} on layer: ${layer}`;
  },
  annotations: {
    readOnlyHint: false,
    untrustedContentHint: false,
  },
});
```

- `inputSchema` is JSON Schema (`type` / `properties` / `required`).
- `execute` is async and returns a **string** result.
- Options (second arg): `signal` (an `AbortSignal` to unregister the tool) and `exposedTo` (array of origins allowed to call it).
- Related surface: `document.modelContext.getTools()`, `executeTool(tool, jsonString, { signal })`, and a `toolchange` event.

**Security annotation worth knowing:** `untrustedContentHint: true` marks a tool's output as untrusted to the agent — use it when the result includes user-generated content (e.g. a to-do item's text) so the agent doesn't treat that text as instructions. `readOnlyHint: true` signals the tool doesn't mutate state.

### When to adopt WebMCP

- **Now, low-risk:** pick a few high-value, non-destructive flows (search/filter, quote request, signup) and register them with the imperative API — the only form in the spec draft. Declarative form annotations are faster to add and work in Chrome's trial, but they may not survive standardization, so keep them additive.
- **Beyond forms** (configurators, multi-step flows): the imperative API is the only option.
- **Don't** rebuild your whole UI around it or block the origin trial's stability on a production-critical path.

## Universal Commerce Protocol (UCP) — agentic commerce

UCP is the **commerce-specific** layer that sits above general agent↔web interaction. It lets an AI agent discover products, negotiate capabilities, run checkout, and pay — without a bespoke per-merchant integration.

**Governance / reach:** an open standard co-developed by **Google and Shopify** (with Etsy, Wayfair, Target, and Walmart as launch co-contributors), launched 2026-01-11 (ucp.dev). On Google it powers **checkout directly on AI Mode in Search and in Gemini (web and mobile app)**, with the merchant as seller of record, plus a **cart-transfer** path that hands a Google-built cart to the merchant's own site; Google's FAQ states that native integration *"does not influence how we rank your offers in product listings"* (developers.google.com/merchant/ucp/faq, updated 2026-10-16). Note that Google's help center uses the name "agentic checkout" for a *different* feature, where an agent buys on the merchant's own website (support.google.com/merchants/answer/16837055). **Latest spec: v2026-08-25** (the first release since v2026-04-08): payment extensions moved from `dev.ucp.shopping.*` to `dev.ucp.common.payment.*`, vendor-agnostic 3DS2 via a new `actions[]` primitive, payment schedules and split payments, independent capability versioning, Web Bot Auth interop, and breaking changes to the fulfillment schema, buyer consent, and profile keys (`signing_keys[]` removed; `keys[]` is canonical) (github.com/Universal-Commerce-Protocol/ucp/releases/tag/v2026-08-25). v2026-04-08 had added the Cart capability (`dev.ucp.shopping.cart`, which Google's consumer "Universal Cart" is built on), Catalog search/lookup, and Identity Linking. **Google's merchant implementation still targets `2026-04-08`** (*"Ensure your system is set up to handle UCP version 2026-04-08"*, developers.google.com/merchant/ucp/guides/overview/cart-overview, updated 2026-10-16) — build against the version Google's docs name, not the newest tag. Still emerging.

Key concepts (useful vocabulary even before you implement):

- **Discovery via `/.well-known/ucp`** — a business publishes a profile: capabilities, services, payment handlers, signing keys.
- **Reverse-domain capability naming** — `{reverse-domain}.{service}.{capability}`, e.g. `dev.ucp.shopping.checkout`, `dev.ucp.shopping.fulfillment`. No central registry; the namespace owner is the governance authority (`dev.ucp.*` is reserved for the UCP body).
- **Transports** — REST (OpenAPI), MCP (OpenRPC), A2A (Agent Card), and embedded (OpenRPC).
- **Server-selects negotiation** — the business computes the intersection of its and the platform's declared capabilities/versions.
- **`UCP-Agent` header** — platforms advertise their profile URL per request, enabling permissionless onboarding.
- **Payment "Trust Triangle"** — Business ↔ Payment Credential Provider ↔ Platform, decoupled so the platform never touches raw card numbers (PCI-scope minimization). **Payment Handlers** (e.g. `com.google.pay`, `dev.shopify.shop_pay`) are provider-authored specs.
- **AP2 Mandates Extension (`dev.ucp.common.payment.ap2_mandate`)** — named `dev.ucp.shopping.ap2_mandate` until UCP v2026-08-25 moved payment extensions into the vertical-agnostic `common.payment` namespace (ucp.dev/latest/specification/payment/extensions/ap2-mandates/). It is the checkout-and-payment authorization *security layer* that plugs **into UCP as an extension** (not a competing protocol). Google's UCP FAQ calls AP2 *"a specialized payment layer within UCP."* AP2 (Agent Payments Protocol) was contributed by Google to the FIDO Alliance on 2026-04-28; FIDO's Payments Technical Working Group, chaired by members from Mastercard and Visa, *"is developing specifications for agent-initiated commerce"* drawing on AP2 and Mastercard's Verifiable Intent — no FIDO-published AP2 spec exists yet (fidoalliance.org/fido-alliance-agentic-ai/, modified 2026-08-20). AP2 defines a **Checkout Mandate** and a **Payment Mandate** — cryptographically signed, non-repudiable proofs that the user authorized a specific transaction; "open" mandates carry the user's constraints for autonomous execution, "closed" mandates authorize one finalized checkout. v0.2 (2026-04-28, still the latest release as of 2026-10-17) added "Human Not Present" autonomous payment flows (github.com/google-agentic-commerce/AP2/releases; ap2-protocol.org).
- **`continue_url`** — graceful degradation: if negotiation fails, hand the buyer off to the normal web checkout.

### When to adopt UCP

- **Watch, don't build** unless you're an ecommerce platform or large merchant in the Google/Shopify ecosystem and tracking the spec deliberately.
- **Do** keep a clean, agent-operable web checkout (the `continue_url` fallback target). A site whose normal checkout works for an agent is the site that benefits first when these standards mature.
- **Monitor** the spec at `ucp.dev` and Merchant Center help for rollout in your category/region.

## Agentic Commerce Protocol (ACP)

ACP is a full-stack agentic-commerce protocol co-created by **Stripe and OpenAI**, launched 2025-09-29. It covers product-discovery feeds and transactional flows for the ChatGPT ecosystem; OpenAI's earlier native "Instant Checkout" surface was deprecated and repositioned — transactional integration now happens via ChatGPT apps. Shopify merchants and Stripe merchants get coverage through their platforms without bespoke integration.

ACP and UCP address overlapping ground (agentic discovery + checkout) from different ecosystem entry points (OpenAI/Stripe vs. Google/Shopify). They are architecturally comparable but not interchangeable; expect eventual convergence pressure as the ecosystem matures.

**Merchant readiness (Stripe).** A Stripe merchant becomes ACP-eligible through Stripe's **Agentic Commerce Suite** — upload a product-catalog feed (the `ProductCatalogImport` API or a Dashboard CSV), configure agents and policies in the Dashboard, and Stripe hosts the ACP-compliant checkout endpoint; payments settle through Checkout Sessions with **Shared Payment Tokens** (charged via PaymentIntents). It is **not** "Payment Links / predefined Price objects" — those are unrelated primitives, a common misconception. ACP itself is PSP-agnostic — you can implement the open spec and use Stripe only for the payment layer. To appear specifically in **ChatGPT Instant Checkout**, a merchant applies to OpenAI; **Shopify and Etsy merchants are pre-approved** via platform agreements.

**Practical stance:** watch, don't hand-roll. If you're already on Shopify or Stripe, coverage arrives via the platform. If you're building a custom commerce stack, monitor both specs before committing to either.

## Agentic Resource Discovery (ARD)

ARD is an open spec for describing and finding an organization's agentic resources — **MCP servers, A2A agents, Agent Skills, plugins, registries, and nested catalogs** — as JSON-LD *entries*. It works in two layers: **static discovery**, where a domain publishes a manifest of entries, and **dynamic discovery**, where registries index those manifests and expose a REST search API. It is a protocol-agnostic layer that sits *above* MCP/A2A/UCP: agents use it to find what a domain offers, then connect through each resource's own protocol.

**Where to publish (v0.91).** The canonical location is **`/.well-known/ard.json`** (a JSON document with an `entries` array), advertised with **`rel="ard"`**. The spec: *"A consumer resolving a domain's entries MUST fetch `/.well-known/ard.json`, and MUST honour a `rel="ard"` link."* The earlier **`/.well-known/ai-catalog.json`** path and `rel="ai-catalog"` are now the *predecessor* names: consumers *"MAY additionally consult"* them, and *"a publisher on the predecessor path SHOULD move to `ard.json`."* The five discovery mechanisms are the well-known URI, in-page JSON-LD markup, a robots.txt `Agentmap:` directive, an HTML `<link rel="ard">` tag, and DNS Service Binding records (primary: github.com/ards-project/ard-spec/blob/main/spec/ard.md §5.1).

**Status:** v0.91, status "Proposal", dated 2026-08-26; the text is still being edited (current as of 2026-10). The previous version, v0.9 (Draft), dated 2026-05-28, is kept at `spec/ard-v0.9.md`. The repo has no tagged releases. Google announced ARD on 2026-06-17 (developers.googleblog.com). Apache 2.0 license. The authors are from Google and Hugging Face; the acknowledgements credit people at AWS, Cisco, Databricks, GitHub, GoDaddy, Google, Microsoft, Nvidia, Salesforce, and Snowflake. Google's announcement credits the Linux Foundation's AI Catalog Working Group for the underlying data model. ADR 0010 (decision of 2026-07-09) keeps the two loosely coupled: *"`ai-catalog` is a publishing format; ARD is a discovery format."*

**Lighthouse:** 13.5.0 (2026-09-18) added an `ard-schema` audit, but it still looks only for the legacy `ai-catalog` names. A port to v0.91 is merged on Lighthouse's `main` (2026-10-09) and unreleased as of 2026-10-17. See [agentic-experiences.md](agentic-experiences.md).

**Important surface note:** publishing an ARD manifest (`ard.json`, or the legacy `ai-catalog.json`) has **no effect on Google Search ranking** (Surface A). This is a Surface B–only signal: it helps AI agents and agent platforms discover your capabilities once they already have your domain; it does not influence how Google's ranking or generative-AI systems retrieve or cite you.

**Practical stance:** very new — no adoption data, and the spec is still a Proposal. For teams already publishing MCP servers or A2A agents, publish at `/.well-known/ard.json` and link it with `rel="ard"`. If you already serve `ai-catalog.json`, move to `ard.json`. Don't contort a v0.91 manifest to pass Lighthouse 13.5.0's legacy-only check; the released audit predates v0.91.

## The relationship between these standards

WebMCP is the **general** agent-interaction layer (any form/tool on any site). UCP is the **commerce** layer that rides on top for discovery, checkout, and payment, with AP2 as its security-authorization extension and ACP as the parallel OpenAI/Stripe-ecosystem counterpart. ARD is the discovery-envelope layer above all of them — a manifest at `/.well-known/ard.json` (plus registries that index those manifests) that lists whatever agentic resources a domain exposes. For most non-commerce sites, WebMCP (plus a clean accessibility tree) is the whole story. Commerce platforms add UCP/ACP. Platforms publishing MCP servers or A2A agents can add ARD.

All of these reduce to the same practical advice as accessibility: **make your site's capabilities explicit, stable, and machine-readable.** Sites that did accessibility well are already most of the way there.

## Related emerging standards

### RSL 1.0 — machine-readable AI licensing

"Really Simple Licensing" is a machine-readable AI-licensing and compensation standard (v1.0 published 2025-12-10). It *complements* robots.txt by adding a licensing/compensation layer on top of crawl permissions; it is distinct from llms.txt and has no search-ranking effect on either surface. The September 2025 launch group included Reddit, Yahoo, Medium, Quora, and Fastly; Cloudflare and Akamai joined for v1.0. Watch for adoption as AI-training licensing norms solidify; implementing it is low-risk if you need to express per-use permissions that robots.txt cannot encode.

### MCP 2026-07-28 — stateless protocol (released)

Heads-up for developers building MCP servers (relevant if you're implementing UCP's MCP transport or any standalone MCP capability): MCP **2026-07-28** shipped as a stable release on 2026-07-28 and is the current protocol version (modelcontextprotocol.io/specification/versioning). It makes the protocol **stateless** — the `initialize`/`initialized` handshake and the `Mcp-Session-Id` header are gone, every request carries its protocol version and client capabilities in `_meta`, and servers MUST implement a new `server/discover` RPC. Streamable HTTP requests must also send `Mcp-Method` and `Mcp-Name` headers. Tasks moved out of the core into the `io.modelcontextprotocol/tasks` extension, and Roots, Sampling, and Logging are deprecated with at least a twelve-month window (modelcontextprotocol.io/specification/2026-07-28/changelog). Servers that need state across calls mint explicit handles and pass them as ordinary tool arguments. UCP's MCP binding links the unversioned MCP spec and pins no protocol version (checked at UCP v2026-08-25), so confirm which MCP revision your platform's agents speak before dropping handshake support; the spec documents backward compatibility with the handshake-based revisions (`2025-11-25` and earlier).
