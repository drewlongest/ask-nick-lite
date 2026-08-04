# ask-nick-2026

A Claude skill that answers questions the way [Nick Saraev](https://www.youtube.com/@nicksaraev) would, grounded in his 2026 YouTube corpus (107+ videos across @nicksaraev and @nicksaraevdaily) with inline citations to the exact videos.

The knowledge base follows the [Cerebras KB architecture](https://www.cerebras.ai/blog/how-we-built-our-knowledge-base): every video is distilled into claims, advice, and quotable passages before embedding; retrieval is hosted (Pinecone, server-side embedding) behind a free read-only search endpoint. New videos are ingested on a recurring schedule, so answers stay current without you doing anything.

## Install

Claude Code:

```bash
mkdir -p ~/.claude/skills/ask-nick-2026
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-nick-2026/main/SKILL.md -o ~/.claude/skills/ask-nick-2026/SKILL.md
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-nick-2026/main/first_principles.md -o ~/.claude/skills/ask-nick-2026/first_principles.md
```

Then in any session: `/ask-nick-2026 how should I price an automation project?`

No API keys, no database, no setup beyond the two files.

## How it works

1. Your Claude loads Nick's distilled first principles (in this repo) so every answer starts from his actual worldview.
2. It queries the hosted index with several phrasings of your question; the endpoint returns the most relevant distilled claims and verbatim passages, each paired with its source video title and URL.
3. A subagent synthesizes the answer in Nick's frame, citing every substantive claim to the exact video.

The endpoint is read-only and rate-limited (30 requests/minute per IP). This is an unofficial fan/study project; answers are an analyst's channeling of Nick's published positions, not Nick himself.
