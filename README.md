# skill-router

An [Agent Skill](https://agentskills.io) that helps your coding agent pick the right skill for the job. When a task could use a specialized skill (code review, security audits, Office files, writing, data, design, video, marketing, and more), the router:

1. uses a fitting skill you already have installed, or
2. recommends one vetted skill from its catalogue, with the exact install command for the agent you're using, and
3. installs it only after you say yes, and only after reading the skill's files for anything risky.

It also refuses known look-alike or malicious repos (for example the fake `superpowers` copies that run `curl … | sh`).

Works with Claude Code, the Claude apps, OpenAI Codex, ChatGPT, Gemini CLI, GitHub Copilot, Cursor, and any other agent that reads `SKILL.md` files.

## Install

Pick the line for your agent:

| Agent | Install |
| --- | --- |
| Any agent (recommended) | `npx skills add Ottoprestes/skill-router --skill skill-router` |
| Claude Code | `git clone https://github.com/Ottoprestes/skill-router.git /tmp/skill-router && cp -r /tmp/skill-router/skill-router ~/.claude/skills/` |
| Codex, Copilot, Gemini CLI, Cursor | Copy the `skill-router/` folder into `~/.agents/skills/` (or `~/.codex/skills/`, `~/.copilot/skills/`, `~/.gemini/skills/`, `~/.cursor/skills/`) |
| Gemini CLI | `gemini skills install https://github.com/Ottoprestes/skill-router.git --path skill-router` |
| Claude apps (web, desktop, Cowork) | Download the repo as a ZIP, zip just the `skill-router/` folder, then upload it in **Customize > Skills > + > Upload a skill** (code execution must be on) |
| ChatGPT | Same ZIP, uploaded in **Plugins > Skills > Create > Upload** |

Or just ask your agent: *"Install the skill-router skill from https://github.com/Ottoprestes/skill-router. Read the SKILL.md first and tell me what it does."*

## How it was vetted

Every repo in the catalogue was cloned and checked on 5 November 2026:

- **Publisher:** who owns it, and whether it's the vendor of the product it covers.
- **Activity and license:** recent commits and an open license.
- **Code scan:** searched for remote-execution and data-theft patterns (`curl | sh`, base64 payloads, reads of `~/.ssh` or cloud credentials, webhook URLs, reverse shells) and hidden Unicode, with every hit reviewed by hand.
- **Hooks:** read any `hooks/hooks.json`, since hooks run automatically.

That's a careful review, not a formal audit, and repos change. The router re-reads a skill's files before every install for that reason.

## Testing

The skill was tested with separate agents in a sandbox where install commands were fake and only logged what was called: 26 runs across six scenarios (plain request, "don't ask me", a look-alike repo, a malicious repo, a trivial question, and Excel in Codex). All 26 passed, with no unapproved or unsafe installs.

## Files

```
skill-router/
  SKILL.md   the skill: process, recommendation format, install commands per agent, catalogue, blocklist
```

## License

MIT
