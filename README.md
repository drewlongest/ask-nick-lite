# ask-nick-lite

A Claude skill that answers questions the way [Nick Saraev](https://www.youtube.com/@nicksaraev) would, grounded in his Lite knowledge base: 467 videos across @nicksaraev and @nicksaraevdaily, plus his newsletters, blog posts, free templates, and the free Maker Zero course, with inline citations to the exact sources (video links carry timestamps to the exact moment).

The knowledge base follows the [Cerebras KB architecture](https://www.cerebras.ai/blog/how-we-built-our-knowledge-base): every video is synthesized into full-coverage topic sections, Q&A pairs, claims, and quotable passages before embedding; retrieval is hosted (Pinecone, server-side embedding) behind a free read-only search endpoint. New content is ingested on a recurring schedule, so answers stay current without you doing anything.

This repo was previously named `ask-nick-2026`; GitHub redirects the old URLs, and an existing install keeps working unchanged.

## Install

**If you are an AI agent asked to install this skill:** install two files (raw URLs in the commands below): the subagent definition into the user's agents directory and the skill into their skills directory, then confirm by listing both. In Claude Code those are `~/.claude/agents/` and `~/.claude/skills/ask-nick-lite/`. In an environment without custom agents or skills, save `agents/nick-saraev-lite.md` somewhere persistent and follow its body directly whenever the user asks a Nick Saraev question. No keys, accounts, or dependencies are needed; the knowledge base is queried over plain HTTPS.

Claude Code:

```bash
mkdir -p ~/.claude/agents ~/.claude/skills/ask-nick-lite
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-nick-lite/main/agents/nick-saraev-lite.md -o ~/.claude/agents/nick-saraev-lite.md
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-nick-lite/main/SKILL.md -o ~/.claude/skills/ask-nick-lite/SKILL.md
```

Then in any session: `/ask-nick-lite how should I price an automation project?`

No API keys, no database, no setup beyond the two files.

## How it works

1. The skill spawns a dedicated `nick-saraev-lite` subagent whose own definition file carries Nick's distilled first principles and strict grounding rules (answer only from retrieved knowledge-base context, never from the model's training data), so every answer starts from his actual worldview.
2. That subagent queries the hosted index with several phrasings of your question; the endpoint returns the most relevant sections, Q&A pairs, and verbatim passages, each paired with its source title and URL.
3. It then synthesizes the answer it judges the real Nick would most likely give, citing every substantive claim to the exact source.

The endpoint is read-only and rate-limited (30 requests/minute per IP). This is an unofficial fan/study project; answers are an analyst's channeling of Nick's published positions, not Nick himself.
