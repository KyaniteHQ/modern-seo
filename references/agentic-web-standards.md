# Agentic Web Standards: WebMCP, UCP/ACP, AP2, ARD

This is **Surface B** (see [the two-surface model in SKILL.md](../SKILL.md)): being *usable by AI agents that act on your site*, as opposed to being *found by Google's ranking systems*. None of this affects Google Search ranking. It is a separate, emerging discipline with its own standards.

The standards below are **proposed/experimental** as of this skill's last review (2026-06). The guidance is: understand them, adopt the low-risk progressive-enhancement pieces for flows you care about, and don't build brittle bespoke integrations against specs that are still changing.

## WebMCP — exposing your page's tools to agents

> "WebMCP is a proposed web standard to help you build and expose structured tools for AI agents. WebMCP provides JavaScript and annotates HTML form elements so that agents know exactly how to interact with page features."

The core idea: instead of an agent guessing how your UI works from the DOM, **your site declares its capabilities**. Tools execute on your page **visibly** (the user sees what happens), and WebMCP is a **progressive enhancement** — it layers on top of working HTML/JS, it doesn't replace it.

WebMCP supports three things:

- **Discovery** — register named tools (`checkout`, `filter_results`) that an agent can find.
- **JSON Schemas** — explicit input/output definitions to reduce hallucination or misunderstanding.
- **State** — a shared understanding of current page context.

**Status:** Developed openly (`webmachinelearning/webmcp`), under active discussion and subject to change. Origin trial **Chrome 149–156**; ships unflagged (enabled for all users) in **Chrome 157** (chromestatus.com/feature/5117755740913664). Testing flag `chrome://flags/#enable-webmcp-testing`. Gated by the `tools` Permissions Policy (defaults to `self`; cross-origin iframes need `allow="tools"`).

### Declarative API (annotate HTML forms)

The lowest-effort entry point. Annotate an existing `<form>`:

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

**Volatility note:** The W3C Community Group spec's Declarative API section is still marked TODO (webmachinelearning.github.io/webmcp/). The HTML-annotation surface (`toolname`/`tooldescription`) is therefore the more volatile part of WebMCP — treat it as progressive enhancement only, not a production dependency.

Rules that the Lighthouse audit enforces (see scoring in [agentic-experiences.md](agentic-experiences.md)):

- `toolname` and `tooldescription` go **together** on the `<form>`. Having one without the other is a schema-validity failure.
- Every input the tool uses needs a unique `name`.
- Add `toolparamdescription` (and an associated `<label>`) to each input; missing these triggers warnings.
- Use action-oriented names (`book_appointment`, not `form1`) and describe **what the tool does and when to use it**.

### Imperative API (register JS tools)

For interactions that aren't a simple form submit:

> `navigator.modelContext` is deprecated in Chrome 150. Use `document.modelContext` instead. (developer.chrome.com/docs/ai/webmcp/imperative-api, updated 2026-06-02)

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

- **Now, low-risk:** add declarative annotations to a few high-value forms (signup, quote request, search/filter). It's additive and degrades gracefully.
- **If you have agent-operable interactions** beyond forms (configurators, multi-step flows): consider the imperative API for those specific tools.
- **Don't** rebuild your whole UI around it or block the origin trial's stability on a production-critical path.

## Universal Commerce Protocol (UCP) — agentic commerce

UCP is the **commerce-specific** layer that sits above general agent↔web interaction. It lets an AI agent discover products, negotiate capabilities, run checkout, and pay — without a bespoke per-merchant integration.

**Governance / reach:** an open standard co-developed by **Google and Shopify** (with Etsy, Wayfair, Target, and Walmart as launch co-contributors), launched 2026-01-11 (ucp.dev). Powers **agentic checkout in Google Search AI Mode and the Gemini app**. The v2026-04-08 spec added Universal Cart, a Catalog capability, and Identity Linking. Still emerging.

Key concepts (useful vocabulary even before you implement):

- **Discovery via `/.well-known/ucp`** — a business publishes a profile: capabilities, services, payment handlers, signing keys.
- **Reverse-domain capability naming** — `{reverse-domain}.{service}.{capability}`, e.g. `dev.ucp.shopping.checkout`, `dev.ucp.shopping.fulfillment`. No central registry; the namespace owner is the governance authority (`dev.ucp.*` is reserved for the UCP body).
- **Transports** — REST (OpenAPI), MCP (OpenRPC), A2A (Agent Card), and embedded (OpenRPC).
- **Server-selects negotiation** — the business computes the intersection of its and the platform's declared capabilities/versions.
- **`UCP-Agent` header** — platforms advertise their profile URL per request, enabling permissionless onboarding.
- **Payment "Trust Triangle"** — Business ↔ Payment Credential Provider ↔ Platform, decoupled so the platform never touches raw card numbers (PCI-scope minimization). **Payment Handlers** (e.g. `com.google.pay`, `dev.shopify.shop_pay`) are provider-authored specs.
- **AP2 Mandates Extension (`dev.ucp.shopping.ap2_mandate`)** — the checkout-and-payment authorization *security layer* that plugs **into UCP as an extension** (not a competing protocol). AP2 (Agent Payments Protocol) was donated by Google to the FIDO Alliance on 2026-04-28 and is now multi-stakeholder, with the FIDO Payments TWG chaired by Mastercard and Visa. It defines a **Checkout Mandate** and a **Payment Mandate** — cryptographically signed, non-repudiable proofs that the user authorized a specific transaction. v0.2 added "Human Not Present" autonomous payment flows. This is the agent-checkout security primitive (ap2-protocol.org).
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

ARD is an open spec for a `/.well-known/ai-catalog.json` file that enumerates an organization's **MCP servers, A2A agents, OpenAPI tools, and nested catalogs** in a single machine-readable document. It is a protocol-agnostic discovery layer that sits *above* MCP/A2A/UCP/OpenAPI — agents and platforms query it to find what capabilities a domain exposes, without needing per-protocol discovery logic.

**Status:** announced 2026-06-17; v0.9 Draft, Apache 2.0 license (github.com/ards-project/ard-spec). Contributors span approximately 11 organizations (AWS, Cisco, Databricks, GitHub, GoDaddy, Google, Hugging Face, Microsoft, Nvidia, Salesforce, Snowflake); the Linux Foundation AI Catalog WG provides the underlying data model.

**Important surface note:** publishing an `ai-catalog.json` has **no effect on Google Search ranking** (Surface A). This is a Surface B–only signal: it helps AI agents and agent platforms discover your capabilities once they already have your domain; it does not influence how Google's ranking or generative-AI systems retrieve or cite you.

**Practical stance:** very new — no adoption data. Watch the spec. For teams already publishing MCP servers or A2A agents, adding a `/.well-known/ai-catalog.json` is low-effort progressive enhancement once the spec stabilizes.

## The relationship between these standards

WebMCP is the **general** agent-interaction layer (any form/tool on any site). UCP is the **commerce** layer that rides on top for discovery, checkout, and payment, with AP2 as its security-authorization extension and ACP as the parallel OpenAI/Stripe-ecosystem counterpart. ARD is the discovery-envelope layer above all of them — a single well-known endpoint that catalogs whatever protocols a domain exposes. For most non-commerce sites, WebMCP (plus a clean accessibility tree) is the whole story. Commerce platforms add UCP/ACP. Platforms publishing MCP servers or A2A agents can add ARD.

All of these reduce to the same practical advice as accessibility: **make your site's capabilities explicit, stable, and machine-readable.** Sites that did accessibility well are already most of the way there.

## Related emerging standards

### RSL 1.0 — machine-readable AI licensing

"Really Simple Licensing" is a machine-readable AI-licensing and compensation standard (v1.0 published 2025-12-10). It *complements* robots.txt by adding a licensing/compensation layer on top of crawl permissions; it is distinct from llms.txt and has no search-ranking effect on either surface. The September 2025 launch group included Reddit, Yahoo, Medium, Quora, and Fastly; Cloudflare and Akamai joined for v1.0. Watch for adoption as AI-training licensing norms solidify; implementing it is low-risk if you need to express per-use permissions that robots.txt cannot encode.

### MCP 2026-07-28 Release Candidate — stateless protocol change

Heads-up for developers building MCP servers (relevant if you're implementing UCP's MCP transport or any standalone MCP capability): the MCP 2026-07-28 Release Candidate (spec locked ~2026-05-21) makes the protocol **stateless** — it removes the `initialize`/`initialized` handshake and the `Mcp-Session-Id` header. The final spec publishes 2026-07-28 (modelcontextprotocol.io/specification). If you are building MCP servers now, design with the stateless model in mind to avoid a migration when the RC finalizes.
