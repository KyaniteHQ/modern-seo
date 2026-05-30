# Agentic Web Standards: WebMCP and UCP

This is **Surface B** (see [the two-surface model in SKILL.md](../SKILL.md)): being *usable by AI agents that act on your site*, as opposed to being *found by Google's ranking systems*. None of this affects Google Search ranking. It is a separate, emerging discipline with its own standards.

Both standards below are **proposed/experimental** as of this skill's last review (2026-05). The guidance is: understand them, adopt the low-risk progressive-enhancement pieces for flows you care about, and don't build brittle bespoke integrations against specs that are still changing.

## WebMCP — exposing your page's tools to agents

> "WebMCP is a proposed web standard to help you build and expose structured tools for AI agents. WebMCP provides JavaScript and annotates HTML form elements so that agents know exactly how to interact with page features."

The core idea: instead of an agent guessing how your UI works from the DOM, **your site declares its capabilities**. Tools execute on your page **visibly** (the user sees what happens), and WebMCP is a **progressive enhancement** — it layers on top of working HTML/JS, it doesn't replace it.

WebMCP supports three things:

- **Discovery** — register named tools (`checkout`, `filter_results`) that an agent can find.
- **JSON Schemas** — explicit input/output definitions to reduce hallucination or misunderstanding.
- **State** — a shared understanding of current page context.

**Status:** Developed openly (`webmachinelearning/webmcp`), under active discussion and subject to change. Origin trial in **Chrome 149**; testing flag `chrome://flags/#enable-webmcp-testing`. Gated by the `tools` Permissions Policy (defaults to `self`; cross-origin iframes need `allow="tools"`).

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

Rules that the Lighthouse audit enforces (see scoring in [agentic-experiences.md](agentic-experiences.md)):

- `toolname` and `tooldescription` go **together** on the `<form>`. Having one without the other is a schema-validity failure.
- Every input the tool uses needs a unique `name`.
- Add `toolparamdescription` (and an associated `<label>`) to each input; missing these triggers warnings.
- Use action-oriented names (`book_appointment`, not `form1`) and describe **what the tool does and when to use it**.

### Imperative API (register JS tools)

For interactions that aren't a simple form submit:

```js
navigator.modelContext.registerTool({
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
- Related surface: `navigator.modelContext.getTools()`, `executeTool(tool, jsonString, { signal })`, and a `toolchange` event.

**Security annotation worth knowing:** `untrustedContentHint: true` marks a tool's output as untrusted to the agent — use it when the result includes user-generated content (e.g. a to-do item's text) so the agent doesn't treat that text as instructions. `readOnlyHint: true` signals the tool doesn't mutate state.

### When to adopt WebMCP

- **Now, low-risk:** add declarative annotations to a few high-value forms (signup, quote request, search/filter). It's additive and degrades gracefully.
- **If you have agent-operable interactions** beyond forms (configurators, multi-step flows): consider the imperative API for those specific tools.
- **Don't** rebuild your whole UI around it or block the origin trial's stability on a production-critical path.

## Universal Commerce Protocol (UCP) — agentic commerce

UCP is the **commerce-specific** layer that sits above general agent↔web interaction. It lets an AI agent discover products, negotiate capabilities, run checkout, and pay — without a bespoke per-merchant integration.

**Governance / reach:** an open standard **co-developed by Google and Shopify**; it powers **agentic checkout in Google Search AI Mode and the Gemini app**. Current spec revision dated 2026-04-08. Still emerging.

Key concepts (useful vocabulary even before you implement):

- **Discovery via `/.well-known/ucp`** — a business publishes a profile: capabilities, services, payment handlers, signing keys.
- **Reverse-domain capability naming** — `{reverse-domain}.{service}.{capability}`, e.g. `dev.ucp.shopping.checkout`, `dev.ucp.shopping.fulfillment`. No central registry; the namespace owner is the governance authority (`dev.ucp.*` is reserved for the UCP body).
- **Transports** — REST (OpenAPI), MCP (OpenRPC), A2A (Agent Card), and embedded (OpenRPC).
- **Server-selects negotiation** — the business computes the intersection of its and the platform's declared capabilities/versions.
- **`UCP-Agent` header** — platforms advertise their profile URL per request, enabling permissionless onboarding.
- **Payment "Trust Triangle"** — Business ↔ Payment Credential Provider ↔ Platform, decoupled so the platform never touches raw card numbers (PCI-scope minimization). **Payment Handlers** (e.g. `com.google.pay`, `dev.shopify.shop_pay`) are provider-authored specs.
- **AP2 Mandates Extension (`dev.ucp.shopping.ap2_mandate`)** — for **autonomous agents**: a cryptographically signed, non-repudiable proof that the user authorized a specific transaction. This is the agent-checkout security primitive.
- **`continue_url`** — graceful degradation: if negotiation fails, hand the buyer off to the normal web checkout.

### When to adopt UCP

- **Watch, don't build** unless you're an ecommerce platform or large merchant in the Google/Shopify ecosystem and tracking the spec deliberately.
- **Do** keep a clean, agent-operable web checkout (the `continue_url` fallback target). A site whose normal checkout works for an agent is the site that benefits first when these standards mature.
- **Monitor** the spec at `ucp.dev` and Merchant Center help for rollout in your category/region.

## The relationship between the two

WebMCP is the **general** agent-interaction layer (any form/tool on any site). UCP is the **commerce** layer that rides on top for discovery, checkout, and payment. For most non-commerce sites, WebMCP (plus a clean accessibility tree) is the whole story. Commerce platforms add UCP.

Both reduce to the same practical advice as accessibility: **make your site's capabilities explicit, stable, and machine-readable.** Sites that did accessibility well are already most of the way there.
