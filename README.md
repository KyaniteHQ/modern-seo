# modern-seo

A standalone Codex/Claude skill for modern SEO strategy grounded in Google's official AI optimization guidance.

This repo packages the `modern-seo` skill as a shareable Git repository so it can be versioned, pushed to GitHub, and installed or copied into another skills directory.

## Contents

- `SKILL.md` - the skill definition and operating guidance
- `references/` - supporting reference notes used by the skill
- `evals/evals.json` - evaluation prompts and expected outputs

## What This Skill Covers

`modern-seo` is a strategy-level SEO skill for questions that span both classical Google Search and generative AI surfaces such as AI Overviews and AI Mode.

It is designed to help with:

- unified SEO guidance for Search and AI search
- mythbusting around `llms.txt`, content chunking, and "AI-style" rewriting
- technical SEO foundations for AI visibility
- non-commodity content strategy
- structured data, business listings, and agent-friendly site design

## Install

Copy this folder into your Codex or Claude skills directory under the name `modern-seo`.

Example:

```bash
cp -R modern-seo /path/to/your/skills/modern-seo
```

The skill entrypoint is [`SKILL.md`](./SKILL.md).

## Development

Edit the skill in [`SKILL.md`](./SKILL.md), update supporting docs in [`references/`](./references), and extend test cases in [`evals/evals.json`](./evals/evals.json).

## Source Basis

The skill is grounded in Google's AI optimization documentation and related primary-source SEO guidance, as referenced in the skill frontmatter and reference files.
