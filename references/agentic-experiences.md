# Agentic Experiences

A rising surface in 2025–2026: AI agents that browse the web on a user's behalf — booking flights, comparing products, filling forms, completing checkouts. These agents read the DOM, the rendered page (sometimes via vision models), and the accessibility tree. They operate sites that humans would also operate.

You don't optimize for agents in a separate dimension. You make your site clearer and more accessible, which serves both humans and agents.

## How browser agents read a page

Most current agents combine some mix of:

- **DOM reading** — they parse the rendered HTML, look for headings, buttons, forms, prices, statuses.
- **Accessibility tree** — they consume the same semantic role information assistive technologies use (button, link, textbox, heading, region).
- **Visual rendering** — for layouts that DOM parsing can't resolve, they may use vision to "see" the page like a user does.
- **Stable selectors** — IDs, ARIA labels, visible text. They prefer stable, meaningful anchors over auto-generated CSS class hashes.

If a human can confidently complete a task on your site without help, an agent usually can too. If the human relies on visual conventions a screen reader couldn't parse, the agent may struggle.

## Agent-friendly patterns

These are the same patterns that win on accessibility audits:

- **Real semantic elements.** `<button>` for buttons, `<a href>` for navigation, `<form>` with `<input>`/`<label>` pairs, `<select>`, `<textarea>`. Not `<div onclick>`, not "button-as-styled-span."
- **Accessible labels.** Every interactive element has a name an assistive technology can read: visible text, an `aria-label`, or a wrapped `<label>`. No mystery icon buttons.
- **Landmark structure.** `<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>` so agents (and screen readers) can jump to the right region.
- **Heading hierarchy.** A real outline: one `<h1>`, then `<h2>` for sections, `<h3>` for sub-sections. Don't skip levels for styling.
- **Stable form names.** `<input name="email">`, not `<input id="x_237a8b">`. Agents look up fields by name; auto-generated IDs that change on every build break them.
- **Visible text matches what's spoken.** Don't hide the canonical label and show a different one to sighted users; agents will believe the DOM.
- **Status and feedback in the DOM.** When a form submits, the success/error message should appear in the DOM where a screen reader (and an agent) can detect it — not only as a transient toast that disappears.
- **Predictable URLs.** Don't bury content behind modal dialogs that have no URL. If an agent receives "go to the contact page," there should be a `/contact` route that loads the contact form.
- **No critical content behind hover-only interactions.** Hover menus, hover tooltips with key info — agents won't reliably trigger these.
- **No CAPTCHA-by-default on common flows.** A reasonable bot defense layer is fine; CAPTCHAs gating the homepage or product pages prevent legitimate agent traffic alongside everything else.

## Anti-patterns that break agents

- Click handlers on `<div>` or `<span>` instead of `<button>` (the agent doesn't know it's clickable).
- Forms that submit only via JavaScript with no `<form action>` and no labeled inputs.
- Date pickers and selects implemented as custom widgets with no ARIA role or keyboard support.
- "Skip ahead" content (e.g., shipping rates, pricing) gated behind a popup that requires geolocation permission.
- Drag-and-drop as the only way to perform an action.
- Multi-step flows that lose state on refresh (no resumable URL).

## Universal Commerce Protocol (UCP) and emerging standards

Google has signaled work on a Universal Commerce Protocol that would let Search agents perform more complex transactional tasks — quote requests, bookings, checkouts — on behalf of users. As of the publication of Google's AI optimization guide, this is **emerging**, not required.

What this means right now:

- **Don't implement UCP-specific code yet.** The spec is not stable and there are no production rewards for early adoption beyond the experiments.
- **Do build sites whose existing flows would be operable by an agent.** When UCP-like standards mature, sites with clean DOM, accessible forms, and predictable URLs will be the ones that work first.
- **Monitor announcements.** The status of UCP and similar protocols (anthropic, OpenAI's "operator," etc.) will evolve; this section will need updating.

## Quick agent-readiness audit

For any flow you care about (contact form, quote request, checkout, account creation), test this:

1. Open the page in a browser with JavaScript enabled and let it fully render.
2. Inspect each interactive element with DevTools' Accessibility tab. Does each one have a non-empty accessible name and a sensible role?
3. Disable mouse use and try to complete the flow with keyboard only (Tab, Shift+Tab, Enter, Space). If you can't, an agent probably can't either.
4. Refresh mid-flow. Does the URL preserve enough state for you to resume?
5. Submit the form. Does the success or error message appear as DOM content?

If steps 2–5 all pass, the page is in good shape for current-generation agents.

## Relationship to accessibility

Agent-readiness ≈ accessibility-readiness. Sites that take accessibility seriously — meaningful labels, semantic elements, keyboard support, ARIA done right — were already agent-friendly before agents existed. The fastest path to agent-readiness for most sites is a serious accessibility pass, not a separate "agent SEO" workstream.
