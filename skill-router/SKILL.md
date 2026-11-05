---
name: skill-router
description: Use when starting a task that a specialized skill or plugin could do better than general knowledge (coding workflow, code review, security audits, Word/Excel/PowerPoint/PDF files, writing, data or scientific analysis, UI design, video or audio, marketing, office roles, building skills), or when the user asks which skill or plugin to install or use, or asks to install a specific skill, plugin or repo.
---

# Skill Router

## Overview

Match the task to the best vetted skill: use one that is already installed, otherwise recommend one from the catalogue below and **install only after the user says yes**. A skill runs with the agent's full permissions, so the catalogue lists only repos that were vetted (publisher, license, activity, code scan for remote-execution and exfiltration patterns, hook review) on 5 Nov 2026.

## When to Use

- The task matches a row in the catalogue below.
- The user asks "is there a skill for X?", "what should I install?", or "set up skills for this project".
- The user asks you to install a specific skill, plugin or repo (for example `npx skills add <owner>/<repo>`).

Do not use when the task is a quick question or one-off answer that needs no specialized procedure, or when a fitting skill is already loaded in this conversation.

## Process

1. **Check what is installed.** Look only at skills installed in the agent you are running in (the skill list in your context, or: Claude Code `/` menu or `claude plugin list`; Codex `/skills`; Gemini CLI `/skills list`; Cursor Customize > Skills; Copilot `.github/skills/` and `~/.copilot/skills/`). If one fits, load it and follow it. Stop here.
2. **Find the match.** Find the task's row in the Catalogue. Pick one skill, the most specific match. If several fit, prefer Official, then Vendor, then Community. If the row's entry for your agent is `n/a`, or no row fits, go to step 7.
3. **Check the blocklist.** If the skill, or a repo the user named, matches anything in Never Install, don't install it. Tell the user the specific reason listed there and offer the catalogue alternative. If the user named a repo that is not in the catalogue and not on the blocklist, do step 5's vetting first and report what you found before any install.
4. **Get consent.**
   - If the user's message already asks you to install a skill for this task ("set up whatever skill is best", "install X"), that request is the yes. State in one line which skill you're installing and from which repo, then go to step 5.
   - Otherwise send the Recommendation message (format below) and wait for the answer before installing anything.
5. **On "yes", vet then install.** Read the skill's `SKILL.md`, plus every file in `scripts/`, `bin/`, `hooks/hooks.json` and `.mcp.json`. If anything downloads and runs code, reads credentials or sends data to an unexpected host, stop and report it. Otherwise run the install command for your agent (see Install by Agent).
6. **Confirm and use.** Check that the skill shows up (reload if the agent requires it), then load it and continue the original task with it.
7. **On "no", or if nothing fits for your agent,** do the task yourself without a skill. Never borrow a skill listed for a different agent, and never copy Claude-only skills into other agents. If the user repeats this kind of task often, offer to write a custom skill with `skill-creator`.

## Recommendation message

Send exactly this shape, filled in.

When you have a shell and can install:

```
Recommended skill: <name> (<badge>) — <repo URL>
Why: <one line linking it to the current task>
Install for <detected agent>: `<exact command>`
What it adds: <instructions only | scripts | hooks that run automatically | MCP server>
Install it now? (yes / no)
```

When you can't install (Claude apps, ChatGPT):

```
Recommended skill: <name> (<badge>) — <repo URL>
Why: <one line linking it to the current task>
How to add it: download the repo (Code > Download ZIP), zip only the skill's own folder, then upload it in <the app's upload path from Install by Agent>.
What it adds: <instructions only | scripts | hooks | MCP server>
Tell me once it's uploaded and I'll use it.
```

In both cases, also do as much of the task as you can without the skill in the same reply, unless the skill is needed to start.

## Install by Agent

| Agent | How to tell | Install method |
| --- | --- | --- |
| Claude Code | `claude` CLI, `~/.claude/` exists | Use the Catalogue's Claude Code column when it has a plugin command (preferred over single-skill installs). Plugin: `claude plugin marketplace add <owner>/<repo>` then `claude plugin install <plugin>@<marketplace>`, then `/reload-plugins`. Single skill: `npx skills add <owner>/<repo> --skill <name> -a claude-code` |
| Codex | `$skill-installer` available, `~/.codex/` exists | `$skill-installer <name or GitHub URL>`, or `codex plugin marketplace add <owner>/<repo>`. Restart Codex afterwards |
| Gemini CLI | `gemini` CLI | `gemini skills install https://github.com/<owner>/<repo>.git --path <folder>` or `gemini extensions install <repo URL>` |
| GitHub Copilot | `copilot` CLI or VS Code | `copilot plugin marketplace add <owner>/<repo>` then `copilot plugin install <plugin>@<marketplace>`, or `npx skills add <owner>/<repo> -a github-copilot` |
| Cursor | `.cursor/` folder | `npx skills add <owner>/<repo> -a cursor` |
| Other terminal agents | anything else with a shell | `npx skills add <owner>/<repo> --skill <name>` (set `DO_NOT_TRACK=1` to turn off its telemetry) |
| Claude apps / ChatGPT (no shell on the user's machine) | no terminal access | You can't install it for them. Tell the user to zip only the skill's own folder and upload it in Claude via **Customize > Skills > + > Upload a skill** (code execution must be on), or in ChatGPT via **Plugins > Skills > Create > Upload** |

## Catalogue

Badges: **Official** (Anthropic, OpenAI, Google, GitHub, Microsoft), **Vendor** (the company whose product it covers), **Community** (known maintainer, open license, clean scan).

| Task signals | Skill | Badge | Repo | Claude Code | Any agent |
| --- | --- | --- | --- | --- | --- |
| Plan, spec, TDD, build a feature end to end | Superpowers | Community | github.com/obra/superpowers | `claude plugin install superpowers@claude-plugins-official` | `gemini extensions install https://github.com/obra/superpowers` |
| Clarify a plan, TDD, debug, architecture, handoff | Matt Pocock's skills | Community | github.com/mattpocock/skills | `claude plugin install mattpocock-skills@claude-plugins-official` | `npx skills@latest add mattpocock/skills` |
| Full dev cycle: specs, incremental work, review | Addy Osmani agent-skills | Community | github.com/addyosmani/agent-skills | marketplace `addyosmani/agent-skills`, plugin `agent-skills@addy-agent-skills` | `npx skills add addyosmani/agent-skills` |
| Plan, work, review and record lessons | Compound Engineering | Community | github.com/EveryInc/compound-engineering-plugin | marketplace `EveryInc/compound-engineering-plugin`, plugin `compound-engineering@compound-engineering-plugin` | `codex plugin marketplace add EveryInc/compound-engineering-plugin` |
| Guided feature build, commits and PRs | feature-dev, commit-commands | Official | github.com/anthropics/claude-plugins-official | `claude plugin install feature-dev@claude-plugins-official` | n/a |
| PR or code review | code-review, pr-review-toolkit | Official | github.com/anthropics/claude-plugins-official | `claude plugin install code-review@claude-plugins-official` | `npx skills add getsentry/skills --skill code-review` |
| Security audit, static analysis, Semgrep, supply chain | Trail of Bits skills | Vendor | github.com/trailofbits/skills | marketplace `trailofbits/skills`, pick in `/plugin` | `codex plugin marketplace add trailofbits/skills` |
| Find bugs, security review, iterate on a PR | Sentry skills | Vendor | github.com/getsentry/skills | marketplace `getsentry/skills`, plugin `sentry-skills@sentry-skills` | `npx skills add getsentry/skills` |
| Language or infra specialist (Python, Kubernetes and so on) | wshobson/agents | Community | github.com/wshobson/agents | marketplace `wshobson/agents`, plugin e.g. `python-development@claude-code-workflows` | `npx skills add wshobson/agents --skill <name>` |
| Test a web app in a real browser | Playwright CLI | Official | github.com/microsoft/playwright-cli | `npm install -g @playwright/cli@latest && playwright-cli install --skills` | same |
| React / Next.js / Vercel | Vercel agent-skills | Vendor | github.com/vercel-labs/agent-skills | `npx skills add vercel-labs/agent-skills -a claude-code` | `npx skills add vercel-labs/agent-skills` |
| Cloudflare Workers, Durable Objects | Cloudflare skills | Vendor | github.com/cloudflare/skills | marketplace `cloudflare/skills`, plugin `cloudflare@cloudflare` | `npx skills add cloudflare/skills` |
| Supabase, Postgres | Supabase agent-skills | Vendor | github.com/supabase/agent-skills | marketplace `supabase/agent-skills`, plugin `supabase@supabase-agent-skills` | `npx skills add supabase/agent-skills` |
| Stripe payments | Stripe skills | Vendor | github.com/stripe/ai | `claude plugin install stripe@claude-plugins-official` | `codex plugin add stripe@openai-curated` |
| Expo / React Native | Expo skills, Callstack skills | Vendor | github.com/expo/skills, github.com/callstackincubator/agent-skills | `claude plugin install expo@claude-plugins-official` | `npx skills@latest add callstackincubator/agent-skills --skill '*'` |
| Terraform, Packer | HashiCorp agent-skills | Vendor | github.com/hashicorp/agent-skills | marketplace `hashicorp/agent-skills`, plugin `terraform@hashicorp` | `npx skills add hashicorp/agent-skills` |
| Neon, Firebase, Azure, Gemini API | Neon, Firebase, Microsoft, Gemini skills | Vendor / Official | github.com/neondatabase/agent-skills, github.com/firebase/skills, github.com/microsoft/skills, github.com/google-gemini/gemini-skills | see repo README | `npx skills add <owner>/<repo>` |
| Fix CI, address PR comments, deploy (Codex) | OpenAI curated skills | Official | github.com/openai/skills | n/a | `$skill-installer gh-fix-ci` |
| Word, Excel, PowerPoint, PDF files | document-skills (docx, xlsx, pptx, pdf) | Official | github.com/anthropics/skills | marketplace `anthropics/skills`, plugin `document-skills@anthropic-agent-skills` | Claude only (license tied to Anthropic terms). Other agents: no vetted Word/Excel/PowerPoint skill, so build the file directly with a library such as python-docx, openpyxl or python-pptx. PDFs in Codex: `$skill-installer pdf` |
| Co-write a document, internal comms | doc-coauthoring, internal-comms | Official | github.com/anthropics/skills | plugin `example-skills@anthropic-agent-skills` | `npx skills add anthropics/skills --skill internal-comms` |
| Make text sound less AI-written | Humanizer | Community | github.com/blader/humanizer | `npx skills add blader/humanizer -a claude-code` | `npx skills add blader/humanizer` |
| Apply a brand voice | brand-voice | Official | github.com/anthropics/knowledge-work-plugins | marketplace `anthropics/knowledge-work-plugins`, plugin `brand-voice@knowledge-work-plugins` | n/a |
| Copywriting, SEO, CRO, pricing, launches, ads | Marketing Skills | Community | github.com/coreyhaines31/marketingskills | `npx skills add coreyhaines31/marketingskills --skill <name> -a claude-code` | same without `-a` |
| Data analysis, SQL, charts, dashboards | data | Official | github.com/anthropics/knowledge-work-plugins | plugin `data@knowledge-work-plugins` | n/a |
| Search across connected tools | enterprise-search | Official | github.com/anthropics/knowledge-work-plugins | plugin `enterprise-search@knowledge-work-plugins` | n/a |
| Biology, chemistry, lab, scientific databases | Scientific Agent Skills | Community | github.com/K-Dense-AI/scientific-agent-skills | `npx skills add K-Dense-AI/scientific-agent-skills --skill <name> -a claude-code` | same without `-a` |
| Bio research pipelines, single-cell RNA | bio-research | Official | github.com/anthropics/knowledge-work-plugins | plugin `bio-research@knowledge-work-plugins` | n/a |
| Finance: models, equity research, reconciliations | Financial services plugins | Official | github.com/anthropics/financial-services | marketplace `anthropics/financial-services`, plugin `financial-analysis@claude-for-financial-services` | n/a |
| ML models, datasets, Hugging Face | Hugging Face skills | Vendor | github.com/huggingface/skills | marketplace `huggingface/skills`, plugin `hf-cli@huggingface-skills` | `gemini extensions install https://github.com/huggingface/skills.git --consent` |
| dbt analytics engineering | dbt agent skills | Vendor | github.com/dbt-labs/dbt-agent-skills | marketplace `dbt-labs/dbt-agent-skills`, plugin `dbt@dbt-agent-marketplace` | `npx skills add dbt-labs/dbt-agent-skills` |
| Distinctive UI / frontend look | frontend-design | Official | github.com/anthropics/claude-plugins-official | `claude plugin install frontend-design@claude-plugins-official` | `npx skills add anthropics/skills --skill frontend-design` |
| Posters, themes, generative art, brand styling | canvas-design, theme-factory, algorithmic-art, brand-guidelines | Official | github.com/anthropics/skills | plugin `example-skills@anthropic-agent-skills` | `npx skills add anthropics/skills --skill <name>` |
| Design critique, accessibility, UX copy, handoff | design | Official | github.com/anthropics/knowledge-work-plugins | plugin `design@knowledge-work-plugins` | n/a |
| UI styles, palettes, font pairings | UI UX Pro Max | Community | github.com/nextlevelbuilder/ui-ux-pro-max-skill | marketplace `nextlevelbuilder/ui-ux-pro-max-skill`, plugin `ui-ux-pro-max@ui-ux-pro-max-skill` | `npx skills add nextlevelbuilder/ui-ux-pro-max-skill` |
| UI designs with Google Stitch | Stitch skills | Official | github.com/google-labs-code/stitch-skills | `npx skills add google-labs-code/stitch-skills -a claude-code` | `npx skills add google-labs-code/stitch-skills` |
| Figma or Canva files | Figma, Canva plugins | Vendor | github.com/anthropics/claude-plugins-official | `claude plugin install figma@claude-plugins-official` (or `canva@…`) | n/a |
| Video in code | Remotion skills, HyperFrames | Vendor | github.com/remotion-dev/skills, github.com/heygen-com/hyperframes | `npx skills add remotion-dev/skills -a claude-code` | `npx skills add heygen-com/hyperframes` |
| Speech, dubbing, music, sound effects | ElevenLabs skills (needs API key) | Vendor | github.com/elevenlabs/skills | `npx skills add elevenlabs/skills -a claude-code` | `npx skills add elevenlabs/skills` |
| Office roles: sales, legal, finance, HR, PM, support, ops | Knowledge Work plugins | Official | github.com/anthropics/knowledge-work-plugins | plugin `<role>@knowledge-work-plugins` | Claude apps: plugin directory |
| Obsidian notes, Bases, Canvas | Obsidian skills | Community | github.com/kepano/obsidian-skills | marketplace `kepano/obsidian-skills`, plugin `obsidian@obsidian-skills` | `npx skills add kepano/obsidian-skills` |
| n8n automations | n8n skills | Community | github.com/czlonkowski/n8n-skills | marketplace `czlonkowski/n8n-skills`, plugin `n8n-mcp-skills@n8n-mcp-skills` | `npx skills add czlonkowski/n8n-skills` |
| Notion pages from meetings or specs | Notion skills | Official | github.com/openai/skills | `claude plugin install notion@claude-plugins-official` | `$skill-installer notion-meeting-intelligence` |
| Write a new skill | skill-creator | Official | github.com/anthropics/skills | `claude plugin install skill-creator@claude-plugins-official` | built into Codex and ChatGPT |
| Build a Claude Code plugin | plugin-dev | Official | github.com/anthropics/claude-plugins-official | `claude plugin install plugin-dev@claude-plugins-official` | n/a |
| Build an MCP server | mcp-builder | Official | github.com/anthropics/skills | plugin `example-skills@anthropic-agent-skills` | `npx skills add anthropics/skills --skill mcp-builder` |
| Scan a third-party skill before installing | Sentry skill-scanner | Vendor | github.com/getsentry/skills | `npx skills add getsentry/skills --skill skill-scanner -a claude-code` | same without `-a` |

Where "marketplace X, plugin Y" appears, run `claude plugin marketplace add X` then `claude plugin install Y`. Inside a session, the same works as `/plugin marketplace add X` and `/plugin install Y`.

## Never Install

- `101-skills/superpowers`, `magentosh/superpowers`, or any "superpowers" repo not owned by `obra`. They are look-alikes whose skills run `curl … | sh`.
- Skills from open-upload registries (ClawHub / OpenClaw) unless you've read every file. That registry hosted the ClawHavoc malware campaign.
- Mega-bundles you can't review: `sickn33/antigravity-awesome-skills` and other collections of thousands of skills.
- `flowkit-labs/skills` `reddit-automation`, which writes disguised promotional posts.
- Ask first, with a warning, before suggesting `thedotmack/claude-mem`: it records every session, and one variant sends data to a hosted server.
- Any repo whose owner doesn't match the catalogue exactly (for example `anthropic-skills` instead of `anthropics/skills`).

## Common Mistakes

| Mistake | Fix |
| --- | --- |
| Installing without asking because the badge is Official | Always send the Recommendation message and wait for "yes". |
| Recommending several skills at once | Recommend one: the most specific match. Mention an alternative only if the user asks. |
| Installing a skill that duplicates one already loaded | Check step 1 first. Never run two skills that do the same job. |
| Running a plugin's install without reading its hooks | Read `hooks/hooks.json`, `.mcp.json` and `bin/` before installing. Hooks run automatically. |
| Using a Claude Code command in another agent | Use the Install by Agent table for the agent you're actually running in. |
| Recommending a skill for a trivial one-off question | Skip the router and just answer. |
| Copying Anthropic's docx/xlsx/pptx/pdf skills into non-Claude agents | Their license limits them to Claude. Use the agent's own equivalent. |
