# Research: productivity, research and video-editing skills (7 Nov 2026)

Candidates for the skill-router catalogue. Each repo was shallow-cloned on 7 Nov 2026 and checked for its latest commit, license, `SKILL.md` count, `hooks.json` / `.mcp.json`, and a grep for remote-exec / exfiltration patterns (`curl|sh`, `base64 -d`, `~/.ssh`, cloud creds, webhook URLs, `/dev/tcp`). Every hit was reviewed by hand. This was a quick check, not the full review in the README: scripts were not read line by line.

**Source note:** I couldn't open the Instagram account @itstundeanthony. instagram.com is blocked in this session's network, and web searches found nothing about the handle on other platforms. The list below comes from open-web research. Send the skill names from those posts and I'll check them the same way.

## Video editing / production

| Skill | Repo | Last commit | License | What it does | Install (Claude Code) | Scan notes | Verdict |
|---|---|---|---|---|---|---|---|
| ffmpeg-skill | github.com/kajisho5/ffmpeg-skill | 2026-11-07 | MIT | 42 local FFmpeg tools (cut, captions, reframe, loudness, platform exports) plus an MCP server; checks its own output. No cloud, no API keys | `claude plugin marketplace add kajisho5/ffmpeg-skill` then install `ffmpeg-skill` | Clean. Captions need `pip install faster-whisper` (local model) | **Add** – best general "edit my footage" pick |
| claude-code-video-toolkit | github.com/digitalsamba/claude-code-video-toolkit | 2026-11-06 | MIT | End-to-end production: narrate, score, generate, compose (Remotion), render; 14 skills | `git clone` (project template) | Only hit: setup doc *suggests* the official `uv` installer (`curl astral.sh … \| sh`). Benign, but it is a pipe-to-shell | **Add (Community)** with a note about the uv step |
| video-editing-skill | github.com/6missedcalls/video-editing-skill | 2026-02-27 | MIT | Bash + FFmpeg + Whisper: trim, jump cuts, Hormozi-style captions, overlays, speed | `npx skills add 6missedcalls/video-editing-skill -a claude-code` | Clean, but quiet since Feb | Optional: simpler alternative for short-form captions |
| OpenMontage | github.com/calesthio/openmontage | 2026-11-03 | **AGPL-3.0** | Agentic video production system, 140 bundled skills | `git clone` + `make` | `media-use` skill tells the agent to run `curl … heygen.ai/cli/install.sh \| bash`. 140 skills is too many to review | **Don't add**: mega-bundle, pipe-to-shell, AGPL |
| Claude-Code-Video-Toolkit (wilwaldon) | github.com/wilwaldon/Claude-Code-Video-Toolkit | 2026-02-25 | none | Curated list (Remotion, Manim, screen recording, FFmpeg) | – | No skills, just links | Reference only |

Already in the catalogue: Remotion, HyperFrames (video in code), ElevenLabs (voice/audio).

## Research / video-to-knowledge

| Skill | Repo | Last commit | License | What it does | Install | Scan notes | Verdict |
|---|---|---|---|---|---|---|---|
| hyperresearch | github.com/jordan-gibbs/hyperresearch | 2026-11-06 | MIT | Deep-research harness for Claude Code / Codex: web, papers, SEC filings, clinical trials, economic data; verifies citations and keeps a persistent searchable wiki | `pip install hyperresearch && hyperresearch install .` | Clean grep, but it installs a Python package. Read `src/` before adding | **Add** – strongest general deep-research option |
| agent-research-skills | github.com/lingzhi227/agent-research-skills | 2026-02-26 | **none** | 31 skills across the academic-paper lifecycle (lit search, deep-research survey, writing, review, slides) | `npx skills add lingzhi227/agent-research-skills -a claude-code` | Clean. No license, inactive since Feb | Hold: no license |
| ai-research-skills | github.com/WenyuChiou/ai-research-skills | 2026-11-04 | MIT | Catalog / harness of 17 research skills (orientation → release, human gates). Skills live in a companion repo | see README | No `SKILL.md` in this repo | Hold: vet the companion repo first |
| video-lens | github.com/kar2phi/video-lens | 2026-11-06 | MIT | YouTube URL → transcript → HTML report (summary, takeaway, key points, timestamped outline, embedded player) | `npx skills add kar2phi/video-lens -a claude-code` | Clean. Needs `youtube-transcript-api`, `yt-dlp` | **Add** – "summarise this video" |
| transcript-critic | github.com/jftuga/transcript-critic | 2026-04-29 | MIT | `/transcribe` audio or a YouTube URL with whisper.cpp, then a critical analysis | `git clone` + edit local paths in its script | Clean, but manual setup | Optional |

Already in the catalogue for research and productivity: Scientific Agent Skills (K-Dense), bio-research, data, enterprise-search, Obsidian, Notion, n8n, doc-coauthoring.

## Suggested catalogue rows

```
| Edit footage: cut, captions, reframe, loudness, exports | ffmpeg-skill | Community | github.com/kajisho5/ffmpeg-skill | marketplace `kajisho5/ffmpeg-skill`, plugin `ffmpeg-skill` | see repo README |
| Produce a narrated/scored video end to end | claude-code-video-toolkit | Community | github.com/digitalsamba/claude-code-video-toolkit | `git clone` (project template) | n/a |
| Deep research with cited, persistent sources | hyperresearch | Community | github.com/jordan-gibbs/hyperresearch | `pip install hyperresearch && hyperresearch install .` | same (`--target codex` for Codex) |
| Summarise a YouTube video into a report | video-lens | Community | github.com/kar2phi/video-lens | `npx skills add kar2phi/video-lens -a claude-code` | `npx skills add kar2phi/video-lens` |
```

Blocklist candidates: `calesthio/openmontage` (pipe-to-shell installer inside a skill, 140-skill bundle). Also, as the README already says, skip mega-bundles such as `sickn33/agentic-awesome-skills` (2,900+ skills).

## Sources
- https://github.com/kajisho5/ffmpeg-skill
- https://github.com/digitalsamba/claude-code-video-toolkit
- https://github.com/6missedcalls/video-editing-skill
- https://github.com/calesthio/openmontage
- https://github.com/wilwaldon/Claude-Code-Video-Toolkit
- https://github.com/jordan-gibbs/hyperresearch
- https://github.com/lingzhi227/agent-research-skills
- https://github.com/WenyuChiou/ai-research-skills
- https://github.com/kar2phi/video-lens
- https://github.com/jftuga/transcript-critic
- https://agentskillshub.top/best/claude-video-skills/
- https://www.kdnuggets.com/5-best-github-repositories-for-ai-agent-skills

## Token-saving skills (added to the catalogue with a "long task" rule)

| Tool | Repo | Last commit | License | What it saves | Scan notes | Verdict |
|---|---|---|---|---|---|---|
| caveman | github.com/JuliusBrussee/caveman | 2026-11-06 | Apache-2.0 / MIT | Output tokens: terse chat replies (author benchmark ~65% fewer; independent tests 15–25% of the bill, because output is a small share of total tokens). Its own rules keep code, commits and docs in normal prose | `~/.ssh` and credentials hits are defensive comments (refusing symlinks). **But** the full plugin adds SessionStart/SubagentStart/UserPromptSubmit hooks, and a `caveman-setup` skill that routes an app's LLM calls through the hosted `gateway.caveman.so` | **Add the single `caveman` skill only** (`npx skills add JuliusBrussee/caveman --skill caveman`). The full plugin goes on the warning list |
| RTK | github.com/rtk-ai/rtk | 2026-11-06 | Apache-2.0 | Shell, build, test and git output compressed before it reaches context (claims 80–99%) | Only hits: the optional `curl install.sh \| sh` in INSTALL.md (use brew instead) and a test fixture. Telemetry off by default | **Add** for long coding tasks |
| context-mode | github.com/mksglu/context-mode | 2026-11-07 | ELv2 (source-available) | Keeps large tool output out of context, keeps a session log, restores it after compaction | Installs hooks on many events; hook code makes no outbound calls beyond github.com links | **Add** as the alternative to RTK, with a hooks note |
| token-reducer | github.com/madhan230205/token-reducer | 2026-05-02 | MIT | Local retrieval-based (RAG) context compression | Ships a context7 MCP that needs an API key; quiet since May | Skip |
| rescue-tokens (valorisa/Claude-Skills) | github.com/valorisa/Claude-Skills | 2026-09-01 | MIT | Verbosity patterns | Clean, small following | Skip; caveman covers it |
| ClaudeSlim | github.com/apolloraines/claudeslim | – | MIT | Proxy that compresses API calls | Couldn't clone. A proxy sees all your traffic | Skip |

Sources: https://github.com/JuliusBrussee/caveman, https://github.com/rtk-ai/rtk, https://github.com/mksglu/context-mode, https://www.implicator.ai/caveman-claude-code-skill-cuts-output-20-your-bill-barely-notices-2/, https://www.deployhq.com/blog/free-github-repos-for-claude-code, https://www.betterclaw.io/blog/skills-that-reduce-token-usage
