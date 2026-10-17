# Agentic Experiences

This is **Surface B** (see the two-surface model in [SKILL.md](../SKILL.md)): making your site *usable by AI agents that act on it*, distinct from being *found by Google's ranking systems*. Nothing here is a ranking tactic.

A rising surface in 2025–2026: AI agents that browse the web on a user's behalf — booking flights, comparing products, filling forms, completing checkouts. These agents read the DOM, the rendered page (often via vision models), and **the accessibility tree** — which Chrome's documentation calls *"their primary data model"* and *"the 'machine-eye view' of your page."*

Google's own agent now runs on phones: Gemini in Chrome's auto browse reached US Android (AI Pro and Ultra, 2026-08-18) and India (2026-10-07), and Gemini Spark uses it in logged-in sessions, *"handing the task back to you"* for payments (2026-07-30; blog.google). Google says its models are *"trained to detect known threats, such as prompt injection"* and that auto browse asks *"for confirmation before completing some sensitive tasks"*; site owners get no new lever, so mobile flows must work for agents.

You don't optimize for agents in a separate dimension from accessibility. You make your site clearer and more accessible, which serves humans and agents alike. As web.dev puts it: *"Everything we suggest to make a site 'agent-ready' also makes sites better for humans."*

For the emerging *standards* (WebMCP, UCP) see [agentic-web-standards.md](agentic-web-standards.md). This file covers the UX/structure fundamentals and how they're measured.

## How browser agents read a page

Most current agents combine some mix of:

- **DOM reading** — they parse rendered HTML for headings, buttons, forms, prices, statuses.
- **Accessibility tree** — the same role/name/state information assistive tech uses (button, link, textbox, heading, region). This is the agent's high-fidelity map of what's interactive.
- **Visual rendering** — for layouts DOM parsing can't resolve, vision models "see" the page like a user. This is why **layout stability matters**: agents use screenshots and coordinates, so a shifting button gets mis-clicked.
- **Stable selectors** — IDs, ARIA labels, visible text. Agents prefer stable, meaningful anchors over auto-generated CSS class hashes.

If a human can confidently complete a task without help, an agent usually can too. If the human relies on visual conventions a screen reader couldn't parse, the agent may struggle.

## Agent-friendly patterns

These are the same patterns that win accessibility audits, plus a few agent-specific notes drawn from web.dev's agent-UX guidance:

- **Real semantic elements.** `<button>` for buttons, `<a href>` for navigation, `<form>` with `<input>`/`<label>` pairs, `<select>`, `<textarea>`. Not `<div onclick>`. If you must use a non-semantic element, give it the right `role` and `tabindex` (e.g. `<div role="button" tabindex="0">`).
- **`cursor: pointer`** on clickable things. web.dev calls it *"a strong signal for actionability."*
- **Accessible labels.** Every interactive element has a name assistive tech can read: visible text, `aria-label`, or a wrapped/`for`-linked `<label>`. No mystery icon buttons. Use `<label for="…">` to bind labels to inputs so the agent understands each field's purpose.
- **Form-field affordances beyond the label.** Give each input the right `type` (`type="email"`, `type="tel"`), an `autocomplete` token (`autocomplete="name"`/`"email"`/`"tel"`/`"organization"`), `inputmode`, and `required` where appropriate. The `autocomplete` token satisfies **WCAG 2.1 SC 1.3.5 (Identify Input Purpose)** and drives browser/password-manager autofill. No primary source ties AI agents to `autocomplete` specifically — the agent's field-purpose signal is the `<label for>` binding above — so do both: `<label for>` for agents, `autocomplete`/`type` for autofill and WCAG compliance.
- **Landmark structure.** `<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>` so agents and screen readers jump to the right region.
- **Heading hierarchy.** A real outline: one `<h1>`, then `<h2>`/`<h3>`. Don't skip levels for styling.
- **Stable form names.** `<input name="email">`, not `<input id="x_237a8b">`. Auto-generated IDs that change every build break agents that look up fields by name.
- **Visible text matches what's spoken.** Don't hide the canonical label and show a different one; agents believe the DOM.
- **Status and feedback in the DOM.** Success/error messages should appear as DOM content (where a screen reader and an agent detect them), not only as a transient toast.
- **Stable layout (low CLS).** A button that moves between identification and interaction causes mis-clicks. Minimize layout shift on the flows agents use.
- **No ghost / transparent overlays.** web.dev warns: *"visual analysis done by the agent might discard nodes that are covered."* Invisible intercepting layers break interaction.
- **Reasonable hit targets.** Interactive elements should have a visible area larger than ~8 square pixels, or visual analysis may filter them out.
- **Predictable URLs.** Don't bury content behind modal dialogs with no URL. "Go to the contact page" implies a `/contact` route exists.
- **No critical content behind hover-only interactions.** Agents won't reliably trigger hover menus/tooltips.
- **No CAPTCHA-by-default on common flows.** Reasonable bot defense is fine; CAPTCHAs gating the homepage or product pages block legitimate agents too.
- **Progressive tool exposure.** Add WebMCP annotations first to helpful, non-destructive flows such as search, signup, contact, booking, or quote requests. Do not expose destructive or irreversible flows (delete account, cancel order, erase data) as agent tools unless the page also has explicit confirmation, visible review, and guardrails.

## Anti-patterns that break agents

- Click handlers on `<div>`/`<span>` with no role (the agent doesn't know it's clickable).
- Forms that submit only via JS with no labeled inputs.
- Custom date pickers/selects with no ARIA role or keyboard support.
- Key info (shipping rates, pricing) gated behind a popup requiring geolocation permission.
- Drag-and-drop as the only way to perform an action.
- Multi-step flows that lose state on refresh (no resumable URL).
- Transparent overlays or off-screen elements intercepting clicks.

## How agent-readiness is measured: Chrome Lighthouse "agentic browsing"

Chrome ships a Lighthouse **"Agentic Browsing" category** for this. It moved from experimental into Lighthouse's **default config in Lighthouse 13.3.0** (2026-05-07; primary: github.com/GoogleChrome/lighthouse/releases/tag/v13.3.0). The current release, **13.5.0** (2026-09-18), has **seven audits** and is slated for Chrome 156 DevTools (primary: github.com/GoogleChrome/lighthouse/releases/tag/v13.5.0); PageSpeed Insights began serving the category around 2026-06-23 (Lighthouse PR #17090). Lighthouse itself labels the category *"still under development and subject to change,"* and Chrome's scoring doc notes that *"Testing this category requires Chrome 150 or later, and WebMCP audits require registering for the WebMCP origin trial"* (updated 2026-05-05). Two things to set expectations:

- **It is not a 0–100 score.** Per Chrome: *"the standards for the agentic web are still emerging, the current focus is to gather data and provide actionable signals rather than a definitive ranking."* Chrome's launch post calls the category *"informational and unbenchmarked"* (developer.chrome.com/blog/agent-ready-toolkit, 2026-06-22). You get a **pass-ratio** (how many readiness checks pass), pass/fail statuses, and informational counts.
- **Audits are deterministic but results can vary**, because dynamic JS tool registration timing, accessibility-tree size/complexity, and layout shift (CLS) all affect what the snapshot captures.

The category has **seven audits**. Lighthouse's config sorts six of them into three groups and lists CLS on its own (primary: `core/config/default-config.js` at tag v13.5.0). Chrome's scoring doc (updated 2026-05-05) still uses older group names and does not mention ARD or the 40-tool warning, so the release is the source for this list.

**A. WebMCP** (see [agentic-web-standards.md](agentic-web-standards.md) for the APIs). These three audits run only in navigation mode (since 13.4.1).
- *WebMCP tools registered* (informational) — lists tools registered via the declarative or imperative API. Since 13.5.0 it **warns when more than 40 tools are registered**; Chrome advises registering tools *"only when they're useful for the current page state"* and unregistering them afterwards (primary: developer.chrome.com/docs/lighthouse/agentic-browsing/registered-webmcp-tools, updated 2026-09-21). The 40 is Lighthouse's recommendation, not a spec limit.
- *WebMCP form coverage* (informational; Chrome's doc calls it "Forms missing declarative WebMCP") — flags `<form>`s lacking both `toolname` and `tooldescription`. Since 13.5.0 it shows a **pass** when every form is annotated; N/A when the page has no forms. (The spec draft dropped these form attributes on 2026-10-08; Chrome's trial and Lighthouse still support them — see agentic-web-standards.md.)
- *WebMCP schema validity* (can fail) — fails when a form has `tooldescription` but no `toolname` (or vice versa), or a required field lacks a `name`; warns (partial score) when a field lacks a description or an optional field lacks a `name`.

**B. Agent Accessibility** ("agents rely on the accessibility tree as their primary data model")
- *Accessibility tree is well-formed* — one pass/fail audit built on a subset of 33 axe accessibility rules (button, link and form-field names, labels, ARIA roles and attributes, focusable content under `aria-hidden`, duplicate ARIA IDs, `autocomplete` validity). Chrome's scoring doc summarizes it as *names and labels*, *tree integrity* (valid roles and parent-child relationships), and *visibility* (interactive content not hidden from the tree).

**Cumulative Layout Shift (CLS)** (listed in the category without a group) — visual stability, critical for coordinate/screenshot-based interaction. (No agent-specific numeric threshold is published; it defers to the general CLS docs.)

**C. Agent Discoverability** (new group in 13.5.0: *"These audits validate that websites expose discoverable resources, documentation, and catalogs for AI agents."*)
- *llms.txt* — fetches `/llms.txt`. A missing file (any 4xx, including 404) returns **Not Applicable** — absence is not a failure and carries no penalty. It fails on a 5xx or a failed fetch, and also when the file exists but has no H1 heading, no Markdown link, or fewer than 50 characters (primary: `core/audits/agentic/llms-txt.js` at tag v13.5.0; Chrome's llms-txt doc, updated 2026-05-05, mentions only the 5xx and 404 cases). Lighthouse's `main` branch also fails on HTTP 429 (merged 2026-10-09, unreleased as of 2026-10-17).
- *ARD manifest* (`ard-schema`, new in 13.5.0) — validates an [Agentic Resource Discovery](agentic-web-standards.md#agentic-resource-discovery-ard) manifest with a port of ARD's official conformance tester. **13.5.0 looks only for the legacy names:** a robots.txt `Agentmap:` line, `<link rel="ai-catalog">`, a `Link: rel="ai-catalog"` header, then `/.well-known/ai-catalog.json`. N/A when none is found; fails when an advertised manifest can't load or has conformance errors. Because ARD v0.91 moved the canonical location to `/.well-known/ard.json`, 13.5.0 reports a site that publishes only there (or only via `rel="ard"`) as N/A. A port to v0.91 (checks `rel="ard"` and `/.well-known/ard.json` first, flags legacy-only manifests) was merged to Lighthouse `main` on 2026-10-09 and is unreleased as of 2026-10-17 (primary: github.com/GoogleChrome/lighthouse/pull/17259).

Chrome's three explicit "how to score well" recommendations (scoring doc, updated 2026-05-05): **adopt WebMCP** for your key flows, keep a **sound accessibility tree** (semantic HTML + proper ARIA), and **optimize for stability** (reduce layout shift).

## Quick agent-readiness audit

For any non-destructive flow you care about (contact form, quote request, checkout, account creation):

1. Open the page, let it fully render.
2. In DevTools' Accessibility tab, inspect each interactive element — does each have a non-empty accessible name and a sensible role?
3. Disable the mouse and complete the flow with keyboard only (Tab, Shift+Tab, Enter, Space). If you can't, an agent probably can't.
4. Refresh mid-flow. Does the URL preserve enough state to resume?
5. Submit the form. Does the success/error message appear as DOM content? Does each field have a `<label for>`, the correct `type`, and an `autocomplete` token (WCAG 1.3.5 + autofill)?
6. (Optional, forward-looking) Run the Lighthouse agentic-browsing category and review the pass-ratio, any schema-validity failures, and the 40-tool warning if you register many WebMCP tools.
7. (Optional, local) Test as an agent would with Chrome DevTools for agents (`chrome-devtools-mcp`). Its `lighthouse_audit` tool runs Lighthouse, including agentic browsing, on the open page. On a Chrome with WebMCP enabled, the `--categoryExperimentalWebmcp` flag adds `list_webmcp_tools` and `execute_webmcp_tool`, so your coding agent can call your tools (github.com/ChromeDevTools/chrome-devtools-mcp docs, 2026-10; developer.chrome.com/blog/agent-ready-toolkit, 2026-06-22).

If steps 2–5 pass, the page is in good shape for current-generation agents.

## A note on llms.txt here (not in ranking)

`llms.txt` belongs to this surface, not to ranking. It's an optional Markdown summary at your domain root that helps agents grasp your site's structure without crawling everything. Lighthouse treats it as optional (N/A when absent), but a file you do publish is checked for an H1 heading, at least one Markdown link, and a minimum length. It does **nothing** for Google Search or AI Overviews — see [mythbusting.md](mythbusting.md) Myth 1. Where it does matter: some AI agent dev-tools (Cursor, Windsurf) consume it by design, using it to orient to a codebase or site before acting — irrelevant to Google ranking, potentially useful for agent consumers. Publish it only if your goal is agent navigability, and keep it honest and current. Either way, machine-readable files (an `llms.txt`, a `/pricing.md`) only help if they're **discoverable** — linked from the site or listed where an agent will look; an unlinked file at the domain root is rarely fetched.

## Relationship to accessibility

Agent-readiness ≈ accessibility-readiness. Sites that take accessibility seriously — meaningful labels, semantic elements, keyboard support, ARIA done right, stable layout — were already agent-friendly before agents existed. The fastest path to agent-readiness for most sites is a serious accessibility pass plus WebMCP annotations on the few flows that matter, not a separate "agent SEO" workstream.
