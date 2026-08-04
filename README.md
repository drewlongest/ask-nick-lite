# ask-nick-2026

A Claude skill that answers questions the way [Nick Saraev](https://www.youtube.com/@nicksaraev) would, grounded in his 2026 YouTube corpus (107+ videos across @nicksaraev and @nicksaraevdaily) with inline citations to the exact videos.

The knowledge base follows the [Cerebras KB architecture](https://www.cerebras.ai/blog/how-we-built-our-knowledge-base): every video is distilled into claims, advice, and quotable passages before embedding; retrieval is hosted (Pinecone, server-side embedding) behind a free read-only search endpoint. New videos are ingested on a recurring schedule, so answers stay current without you doing anything.

## Install

**If you are an AI agent asked to install this skill:** install two files (raw URLs in the commands below): the subagent definition into the user's agents directory and the skill into their skills directory, then confirm by listing both. In Claude Code those are `~/.claude/agents/` and `~/.claude/skills/ask-nick-2026/`. In an environment without custom agents or skills, save `agents/nick-saraev-2026.md` somewhere persistent and follow its body directly whenever the user asks a Nick Saraev question. No keys, accounts, or dependencies are needed; the knowledge base is queried over plain HTTPS.

Claude Code:

```bash
mkdir -p ~/.claude/agents ~/.claude/skills/ask-nick-2026
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-nick-2026/main/agents/nick-saraev-2026.md -o ~/.claude/agents/nick-saraev-2026.md
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-nick-2026/main/SKILL.md -o ~/.claude/skills/ask-nick-2026/SKILL.md
```

Then in any session: `/ask-nick-2026 how should I price an automation project?`

No API keys, no database, no setup beyond the two files.

## How it works

1. The skill spawns a dedicated `nick-saraev-2026` subagent whose own definition file carries Nick's distilled first principles and strict grounding rules (answer only from retrieved knowledge-base context, never from the model's training data), so every answer starts from his actual worldview.
2. That subagent queries the hosted index with several phrasings of your question; the endpoint returns the most relevant distilled claims and verbatim passages, each paired with its source video title and URL.
3. It then synthesizes the answer it judges the real Nick would most likely give, citing every substantive claim to the exact video.

The endpoint is read-only and rate-limited (30 requests/minute per IP). This is an unofficial fan/study project; answers are an analyst's channeling of Nick's published positions, not Nick himself.
