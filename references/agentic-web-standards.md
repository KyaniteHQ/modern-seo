# Agentic Web Standards: WebMCP, UCP/ACP, AP2, ARD

This is **Surface B** (see [the two-surface model in SKILL.md](../SKILL.md)): being *usable by AI agents that act on your site*, as opposed to being *found by Google's ranking systems*. None of this affects Google Search ranking. It is a separate, emerging discipline with its own standards.

The standards below are **proposed/experimental** as of this skill's last review (2026-10). The guidance is: understand them, adopt the low-risk progressive-enhancement pieces for flows you care about, and don't build brittle bespoke integrations against specs that are still changing.

## WebMCP — exposing your page's tools to agents

> "WebMCP is a proposed web standard to help you build and expose structured tools for AI agents. WebMCP provides JavaScript and annotates HTML form elements so that agents know exactly how to interact with page features, to support a user's experience."

The core idea: instead of an agent guessing how your UI works from the DOM, **your site declares its capabilities**. Tools execute on your page **visibly** (the user sees what happens), and WebMCP is a **progressive enhancement** — it layers on top of working HTML/JS, it doesn't replace it.

WebMCP supports three things:

- **Discovery** — register named tools (`checkout`, `filter_results`) that an agent can find.
- **JSON Schemas** — explicit input/output definitions to reduce hallucination or misunderstanding.
- **State** — a shared understanding of current page context.

**Status:** Developed openly (`webmachinelearning/webmcp`), under active discussion and subject to change. Chrome's origin trial runs **Chrome 149–162**; it was extended from 156 through an Intent to Extend Experiment approved on 2026-09-28 (chromestatus.com/feature/5117755740913664). **No ship milestone is set and no Intent to Ship has been filed.** On 2026-09-29 the Chrome team wrote that it is "hoping to move to I2S [Intent to Ship] within the extension window" (blink-dev, "Intent to Extend Experiment: WebMCP"). Chrome 162 reaches Stable on 2027-02-02; Chrome now ships a Stable milestone every two weeks from Chrome 153, so read milestone numbers against chromiumdash.appspot.com rather than counting four-week cycles. Testing flag `chrome://flags/#enable-webmcp-testing`. Gated by the `tools` Permissions Policy: it defaults to `self`, cross-origin iframes need `allow="tools"`, and a `Permissions-Policy: tools=()` header turns WebMCP off for a page and its frames (developer.chrome.com/docs/ai/webmcp, updated 2026-10-07).

**Limits Chrome states** (developer.chrome.com/docs/ai/webmcp, updated 2026-10-07): the API is "primarily designed for local browser workflows with a human in the loop" (headless use "may be possible"), and "Clients and browsers must visit a site directly to know if it has callable tools." WebMCP does not advertise your tools to an agent that has not loaded the page.

**Who calls these tools** (current as of 2026-10). Besides Chrome: Microsoft lists a WebMCP origin trial in its Edge 150 release notes (learn.microsoft.com/en-us/microsoft-edge/web-platform/release-notes/150). The ChatGPT desktop app uses WebMCP for "site tools" in its built-in browser, but only imperative tools in the top-level page: *"Tools defined through HTML form attributes aren't available as site tools"* (learn.chatgpt.com/docs/webmcp). Meta's Ray-Ban Display glasses call tools in web apps, *"off by default and enabled per device"* (wearables.developer.meta.com/docs/develop/webapps/agent-tools/, updated 2026-10-12). Brave's support in Leo is experimental and flag-gated; Firefox and Safari have only standards-position threads (spec repo implementation-status.md, added 2026-08-12). Gemini in Chrome is not listed, and Chrome says its WebMCP test extensions "are separate from Gemini in Chrome."

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
const controller = new AbortController(); // controller.abort() later unregisters the tool

await document.modelContext.registerTool({
  name: 'book_flight',
  description: 'Book a flight for the user with confirmed flight details.',
  inputSchema: {
    type: 'object',
    properties: {
      flightId: { type: 'string', description: 'ID of the flight to book' },
      passengers: { type: 'number', description: 'Number of tickets to purchase' },
    },
    required: ['flightId', 'passengers'],
  },
  annotations: {
    readOnlyHint: false,         // it changes state
    consequentialHint: true,     // real-world purchase: lets the agent or browser confirm with the user first
    untrustedContentHint: false, // output is written by the site (set true for UGC, reviews, external data)
    debugging: false,            // an end-user tool, not a developer-only one (Chrome 156+)
  },
  execute: async ({ flightId, passengers }, { signal }) => {
    const res = await fetch('/api/bookings', {
      method: 'POST',
      body: JSON.stringify({ flightId, passengers }),
      signal, // aborted if the user or agent cancels
    });
    if (!res.ok) return `Flight ${flightId} is no longer available. Search for flights again.`;
    return `Booked ${passengers} passenger(s) on flight ${flightId}.`;
  },
}, { signal: controller.signal });
```

- `inputSchema` is JSON Schema (`type` / `properties` / `required`). Optional `title` labels the tool in browser UI; agents read `description`.
- `registerTool()` returns a promise. Options: `signal` (abort it to unregister the tool) and `exposedTo` (secure origins allowed to call it; the default is same-origin only).
- `execute(input, { signal })`: stop work when `signal` aborts (pass it to `fetch()`), and return a string or any JSON-serializable value (spec PR #247). Since Chrome 153, unregistering a tool no longer cancels executions already running (spec PR #248).
- Caller side (in-page agents): `getTools()` lists tools alphabetically, with cross-origin tools only if requested via `fromOrigins` and exposed by their owner. `executeTool(tool, inputObject, { signal })` takes a plain object; Chrome marks JSON-string input deprecated from Chrome 155. `toolchange` fires when the list changes (spec PRs #223, #246).
- Experimental framework helpers: React `use-webmcp-tool`, Angular, Vue `@vueuse/core` `useWebMCP`, and `webmcp-types` typings.

(developer.chrome.com/docs/ai/webmcp/imperative-api, updated 2026-10-16)

**Security and sizing** (developer.chrome.com/docs/ai/webmcp/secure-tools, updated 2026-09-01; spec PRs #217, #253). The four annotations default to `false`. Set `untrustedContentHint` when output includes user-generated or external content, so the agent treats it as data rather than instructions. Set `readOnlyHint` on tools that change nothing, and `consequentialHint` on high-stakes or irreversible actions, *"so the agent or browser can request user confirmation before execution."* `debugging: true` (Chrome 156+) marks developer-only tools that end-user agents can filter out. Hints are not enforcement: OpenAI says a tool's *"claim that it only reads data isn't proof of what it does"* (learn.chatgpt.com/docs/webmcp), so keep your own server-side checks. Chrome's character budgets are 500 per tool description, 150 per parameter description, 30 per tool or parameter name, and 1.5K per tool output, with *"some variation across agents."* Lighthouse warns above 40 tools; see [agentic-experiences.md](agentic-experiences.md).

### When to adopt WebMCP

- **Now, low-risk:** pick a few high-value, non-destructive flows (search/filter, quote request, signup) and register them with the imperative API — the only form in the spec draft. Declarative form annotations are faster to add and work in Chrome's trial, but they may not survive standardization, so keep them additive.
- **Beyond forms** (configurators, multi-step flows): the imperative API is the only option.
- **Plan tools from user journeys, not forms.** Chrome's guide: define the user goal and starting state, role-play the conversation to find each tool, let the agent ask for missing parameters, return specific, recoverable errors ("Search for flights first"), then add evals and telemetry (developer.chrome.com/docs/ai/webmcp/build-tools, 2026-08-26).
- **Keep each tool within one page.** A tool call that navigates ends without returning output to the agent. A continuations explainer (spec repo, 2026-10-02) proposes cross-page tools such as multi-page checkout; it is only a proposal.
- **Don't** rebuild your whole UI around it or block the origin trial's stability on a production-critical path.

## Universal Commerce Protocol (UCP) — agentic commerce

UCP is the **commerce-specific** layer that sits above general agent↔web interaction. It lets an AI agent discover products, negotiate capabilities, run checkout, and pay — without a bespoke per-merchant integration.

**Governance / reach:** an open standard co-developed by **Google and Shopify** (with Etsy, Wayfair, Target, and Walmart as launch co-contributors), launched 2026-01-11 (ucp.dev). On Google it powers **checkout directly on AI Mode in Search and in Gemini (web and mobile app)**, with the merchant as seller of record, plus a **cart-transfer** path that hands a Google-built cart to the merchant's own site; Google's FAQ states that native integration *"does not influence how we rank your offers in product listings"* (developers.google.com/merchant/ucp/faq, updated 2026-10-16). Note that Google's help center uses the name "agentic checkout" for a *different* feature, where an agent buys on the merchant's own website (support.google.com/merchants/answer/16837055). **Latest spec: v2026-08-25** (the first release since v2026-04-08): payment extensions moved from `dev.ucp.shopping.*` to `dev.ucp.common.payment.*`, vendor-agnostic 3DS2 via a new `actions[]` primitive, payment schedules and split payments, independent capability versioning, Web Bot Auth interop, and breaking changes to the fulfillment schema, buyer consent, and profile keys (`signing_keys[]` removed; `keys[]` is canonical) (github.com/Universal-Commerce-Protocol/ucp/releases/tag/v2026-08-25). v2026-04-08 had added the Cart capability (`dev.ucp.shopping.cart`, which Google's consumer "Universal Cart" is built on), Catalog search/lookup, and Identity Linking. **Google's merchant implementation still targets `2026-04-08`** (*"Ensure your system is set up to handle UCP version 2026-04-08"*, developers.google.com/merchant/ucp/guides/overview/cart-overview, updated 2026-10-16) — build against the version Google's docs name, not the newest tag. Still emerging.

**Beyond retail (watch; nothing to implement yet):** UCP formed a Food Technical Council (2026-07-16: Block (Square), DoorDash, Google, Toast, Uber Eats), a Lodging Technical Council (2026-08-11: Amadeus, Booking.com, Expedia, Google, Hilton, Marriott, Trip.com), and a Payments Technical Council (2026-09-02: Adyen, Ant International, Coinbase, Global Payments, Google, PayPal, Shopify, Stripe) (ucp.dev/documentation/announcements/). The Lodging council's **Booking capability (`dev.ucp.lodging.booking`)**, announced 2026-09-25, is UCP's first vertical outside shopping. It is draft only, absent from the `latest` release, and the spec warns it *"may undergo breaking changes"* (ucp.dev/draft/specification/lodging/booking/). No Food spec exists yet. Google's UCP pages for hotels (developers.google.com/hotels/ucp) and food ordering (developers.google.com/actions-center/verticals/ordering/ucp) are waitlists that promise *"Detailed onboarding and specs coming soon"* (current as of 2026-10).

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

- **Watch, don't hand-roll the full protocol** unless you're an ecommerce platform or a large merchant tracking the spec deliberately. Platform merchants should ask their platform about its UCP plans, as Google's onboarding help advises (support.google.com/merchants/answer/16992327, current as of 2026-10).
- **Custom stack? Cart transfer is the supported low-effort entry.** One `CreateCart` endpoint receives a cart the shopper built on Google and *"must respond with a continue_url directing the user to their pre-populated cart on your site"*; native checkout, payment handlers, and order lifecycle are not required (developers.google.com/merchant/ucp/implementation/2026-04-08/cart-api, updated 2026-10-09; cart-overview, updated 2026-10-16). Merchant Center's UCP integration hub (a pilot, with a sandbox) calls it a fast track that *"doesn't require full product feed setup, product warnings, or customer support information"*; native checkout adds order-status webhooks and a UCP Program Addendum (developers.google.com/merchant/ucp/guides/tools/merchant-center/integration, updated 2026-10-15). Both need Google's approval. The hub is rolling out in the US, with Australia and Canada next year.
- **Why it matters:** Universal Cart, which Google announced on 2026-10-15 for Search (including AI Mode) and the Gemini app in the US, lets shoppers pay with Google Pay or *"transfer your items directly to the merchant's cart"*; the merchant stays seller of record (blog.google/products-and-platforms/products/shopping/universal-cart-launch/).
- **Do** keep a clean, agent-operable web checkout. The `continue_url` fallback and a transferred cart both land there.
- **Monitor** `ucp.dev` and Merchant Center help for rollout in your category and region.

## Agentic Commerce Protocol (ACP)

ACP is a full-stack agentic-commerce protocol co-created by **Stripe and OpenAI**, launched 2025-09-29. It covers product-discovery feeds and transactional flows for the ChatGPT ecosystem; OpenAI's earlier native "Instant Checkout" surface was deprecated and repositioned — transactional integration now happens via ChatGPT apps. Shopify merchants and Stripe merchants get coverage through their platforms without bespoke integration.

ACP and UCP address overlapping ground (agentic discovery + checkout) from different ecosystem entry points (OpenAI/Stripe vs. Google/Shopify). They are architecturally comparable but not interchangeable; expect eventual convergence pressure as the ecosystem matures. Stripe, ACP's co-creator, has also sat on UCP's Governing Council since 2026-04-28 and on its Payments Technical Council since 2026-09-02 (ucp.dev/documentation/announcements/).

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
