# modern-seo

A portable [Agent Skill](https://code.claude.com/docs/en/skills) for modern SEO and agentic-web strategy, grounded in primary-source guidance from Google and Chrome.

The skill is a folder with a [`SKILL.md`](./SKILL.md) (YAML frontmatter: `name` + `description`) plus supporting [`references/`](./references). That folder format is a cross-vendor convention, so this one skill installs into Claude Code, the Claude API, claude.ai, OpenCode, Codex, and other compatible agents.

## What this skill covers

`modern-seo` is the comprehensive, strategy-level lens for questions that span classical Google Search and the generative-AI surfaces on top of it. Its load-bearing idea is the **two-surface model**:

- **Surface A — getting found in Google Search and its AI features** (AI Overviews, AI Mode). This is still classic SEO; there is no separate "AI ranking."
- **Surface B — being usable by AI agents that browse and act on your site** (WebMCP, the Lighthouse agentic-browsing category, the Universal Commerce Protocol). A genuinely new discipline, not a ranking hack.

Keeping the two straight resolves most "AI SEO" confusion — e.g. `llms.txt` is useless for ranking (Surface A) but has an optional role for agents (Surface B).

It helps with: unified SEO guidance for Search and AI search, mythbusting (`llms.txt`, content chunking, "AI-style" rewriting), technical foundations, non-commodity content and E-E-A-T, structured data, business listings, and agent-friendly site design.

## Install

### Any agent — one command (recommended)

Because this repo is a standard skill (a `SKILL.md` at the root), the open
[`skills`](https://github.com/vercel-labs/skills) CLI installs it into whatever
agents you have:

```bash
npx skills add KyaniteHQ/modern-seo
```

It auto-detects installed agents — Claude Code, Cursor, Codex, OpenCode, Gemini
CLI, GitHub Copilot, Windsurf, and [70+ others](https://github.com/vercel-labs/skills#supported-agents) — and copies the
whole skill folder (`SKILL.md` + `references/`). Requires Node ≥ 18.

```bash
npx skills add KyaniteHQ/modern-seo -g            # global (all projects), not just this one
npx skills add KyaniteHQ/modern-seo -a claude-code # install to a specific agent only
npx skills update modern-seo                        # update later
npx skills remove modern-seo                         # uninstall
```

Equivalent cross-agent installers work too (same root-`SKILL.md` convention):

```bash
npx openskills install KyaniteHQ/modern-seo   # numman-ali/openskills (Node ≥ 20.6)
npx skillkit add KyaniteHQ/modern-seo         # rohitg00/skillkit (translates to 46 agents)
```

### Claude Code — via the marketplace

```
/plugin marketplace add KyaniteHQ/agent-skills
/plugin install modern-seo@kyanite-skills
```

Run `/skills` to confirm it loaded. Update later with `/plugin marketplace update`.

### Claude Code + OpenCode — manual clone (one command, both tools)

OpenCode reads `~/.claude/skills` natively, so cloning into the Claude personal skills directory covers both:

```bash
git clone https://github.com/KyaniteHQ/modern-seo.git ~/.claude/skills/modern-seo
```

- **Claude Code**: run `/skills` to verify.
- **OpenCode**: ask "what skills are available?" to verify.

For a **project-scoped** install (committed to a repo, shared with your team), put it at `.claude/skills/modern-seo/` instead — both tools read that too.

### Codex (OpenAI)

Codex reads skills from `.agents/skills` (repo scope) and `~/.agents/skills` (user scope):

```bash
git clone https://github.com/KyaniteHQ/modern-seo.git ~/.agents/skills/modern-seo
```

### claude.ai (web)

Download this repo as a `.zip`, then upload it under **Settings → Features** (Custom Skills; requires a plan with code execution enabled). The folder inside the zip must contain `SKILL.md` at its root.

### Hermes Agent (Nous Research)

Hermes reads skills from `~/.hermes/skills/`:

```bash
git clone https://github.com/KyaniteHQ/modern-seo.git ~/.hermes/skills/modern-seo
```

> Note: this path applies to Nous Research's **Hermes Agent** CLI, which does not read the `.claude`/`.agents` directories — it needs its own copy. If you mean a different "Hermes," the folder-clone pattern still applies; just point it at that tool's skills directory.

### Any other agent

Drop the folder into whatever directory your agent scans for skills. The skill is self-contained — `SKILL.md` + `references/`, no build step, no dependencies.

## Updating

- **`npx skills` installs**: `npx skills update modern-seo` (or `npx openskills update` / `npx skillkit update` for those tools).
- **Marketplace installs**: `/plugin marketplace update` in Claude Code pulls the latest released version.
- **Manual clones**: `git pull` inside the cloned directory.

```bash
git -C ~/.claude/skills/modern-seo pull
```

## Verifying it loaded

| Tool | Check |
|---|---|
| Claude Code | `/skills`, or ask "what skills are available?" |
| OpenCode | Ask "what skills are available?" |
| Codex | The skill appears in available skills; invoke via its name |
| claude.ai | The skill is listed under Settings → Features |

## Contents

- [`SKILL.md`](./SKILL.md) — the skill definition and operating guidance (the two-surface model + core principles)
- [`references/`](./references) — supporting reference notes (technical foundations, content strategy, AI-search mechanics, structured data, business/local, mythbusting, agentic experiences, agentic web standards)
- [`.claude-plugin/plugin.json`](./.claude-plugin/plugin.json) — Claude plugin manifest (canonical version)

## Versioning

This repo is the single source of truth for the skill's version. The version lives in [`.claude-plugin/plugin.json`](./.claude-plugin/plugin.json) and is mirrored by a matching git tag (e.g. `v2.0.0`). The [`KyaniteHQ/agent-skills`](https://github.com/KyaniteHQ/agent-skills) marketplace references this repo via an unpinned `github` source, so it tracks `main` and always offers the latest released version.

To cut a release: bump `version` in `plugin.json`, update the "Sources and versioning" section in `SKILL.md`, commit, then tag (`git tag -a vX.Y.Z -m "…" && git push origin vX.Y.Z`).

## Development

Edit the skill in [`SKILL.md`](./SKILL.md) and update supporting docs in [`references/`](./references). This repo does not maintain a bundled eval suite; review changes through manual invocation on real SEO or agentic-web tasks. Keep the frontmatter to `name` + `description` (description under ~1024 characters) for maximum cross-agent portability.

## Source basis

Grounded in Google's AI optimization guide, Chrome's agentic-browsing (Lighthouse) and WebMCP documentation, web.dev's agent-friendly UX guidance, and the Universal Commerce Protocol spec. Full source list is in the "Sources and versioning" section of [`SKILL.md`](./SKILL.md).
