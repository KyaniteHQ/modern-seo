# Agentic Experiences

This is **Surface B** (see the two-surface model in [SKILL.md](../SKILL.md)): making your site *usable by AI agents that act on it*, distinct from being *found by Google's ranking systems*. Nothing here is a ranking tactic.

A rising surface in 2025–2026: AI agents that browse the web on a user's behalf — booking flights, comparing products, filling forms, completing checkouts. These agents read the DOM, the rendered page (often via vision models), and **the accessibility tree** — which Chrome's documentation calls *"their primary data model"* and *"the 'machine-eye view' of your page."*

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

## Anti-patterns that break agents

- Click handlers on `<div>`/`<span>` with no role (the agent doesn't know it's clickable).
- Forms that submit only via JS with no labeled inputs.
- Custom date pickers/selects with no ARIA role or keyboard support.
- Key info (shipping rates, pricing) gated behind a popup requiring geolocation permission.
- Drag-and-drop as the only way to perform an action.
- Multi-step flows that lose state on refresh (no resumable URL).
- Transparent overlays or off-screen elements intercepting clicks.

## How agent-readiness is measured: Chrome Lighthouse "agentic browsing"

Chrome ships a Lighthouse **"Agentic Browsing" category** for this. It moved from experimental into Lighthouse's **default config in Lighthouse 13.3.0** (~2026-05-07; primary: github.com/GoogleChrome/lighthouse/releases/tag/v13.3.0); PageSpeed Insights inherited it shortly after (~mid-to-late May 2026). Two things to set expectations:

- **It is not a 0–100 score.** Per Chrome: *"the standards for the agentic web are still emerging, the current focus is to gather data and provide actionable signals rather than a definitive ranking."* You get a **pass-ratio** (how many readiness checks pass), pass/fail statuses, and informational counts.
- **Audits are deterministic but results can vary**, because dynamic JS tool registration timing, accessibility-tree size/complexity, and layout shift (CLS) all affect what the snapshot captures.

The checks fall into three groups:

**A. WebMCP integration** (see [agentic-web-standards.md](agentic-web-standards.md) for the APIs)
- *Registered WebMCP tools* (informational) — lists tools registered via the declarative or imperative API.
- *Forms missing declarative WebMCP* (informational) — flags `<form>`s lacking both `toolname` and `tooldescription`.
- *WebMCP schema validity* (can fail) — fails when a form has `tooldescription` but no `toolname` (or vice-versa), or a required field lacks a `name`; warns when an optional named field lacks `toolparamdescription` or a `<label>`.

**B. Agent-centric accessibility** ("agents rely on the accessibility tree as their primary data model")
- *Names and labels* — every interactive element has a programmatic name.
- *Tree integrity* — roles and parent-child relationships are valid.
- *Visibility* — interactive content is not hidden from the accessibility tree.

**C. Stability and discoverability**
- *Cumulative Layout Shift (CLS)* — visual stability, critical for coordinate/screenshot-based interaction. (No agent-specific numeric threshold is published; it defers to the general CLS docs.)
- *llms.txt* — checks for a machine-readable summary at the domain root. A missing file (404) returns **Not Applicable** — absence is not a failure and carries no penalty. A 5xx server error on the path is the only failure condition (primary: developer.chrome.com/docs/lighthouse/agentic-browsing/llms-txt).

Chrome's three explicit "how to score well" recommendations: **adopt WebMCP** for your key flows, keep a **sound accessibility tree** (semantic HTML + proper ARIA), and **optimize for stability** (reduce layout shift).

## Quick agent-readiness audit

For any flow you care about (contact form, quote request, checkout, account creation):

1. Open the page, let it fully render.
2. In DevTools' Accessibility tab, inspect each interactive element — does each have a non-empty accessible name and a sensible role?
3. Disable the mouse and complete the flow with keyboard only (Tab, Shift+Tab, Enter, Space). If you can't, an agent probably can't.
4. Refresh mid-flow. Does the URL preserve enough state to resume?
5. Submit the form. Does the success/error message appear as DOM content?
6. (Optional, forward-looking) Run the Lighthouse agentic-browsing category and review the pass-ratio and any schema-validity failures.

If steps 2–5 pass, the page is in good shape for current-generation agents.

## A note on llms.txt here (not in ranking)

`llms.txt` belongs to this surface, not to ranking. It's an optional Markdown summary at your domain root that helps agents grasp your site's structure without crawling everything. Lighthouse treats it as optional (N/A when absent). It does **nothing** for Google Search or AI Overviews — see [mythbusting.md](mythbusting.md) Myth 1. Where it does matter: some AI agent dev-tools (Cursor, Windsurf) consume it by design, using it to orient to a codebase or site before acting — irrelevant to Google ranking, potentially useful for agent consumers. Publish it only if your goal is agent navigability, and keep it honest and current.

## Relationship to accessibility

Agent-readiness ≈ accessibility-readiness. Sites that take accessibility seriously — meaningful labels, semantic elements, keyboard support, ARIA done right, stable layout — were already agent-friendly before agents existed. The fastest path to agent-readiness for most sites is a serious accessibility pass plus WebMCP annotations on the few flows that matter, not a separate "agent SEO" workstream.
