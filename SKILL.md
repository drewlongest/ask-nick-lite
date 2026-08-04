---
name: ask-nick-2026
description: Answer a question the way Nick Saraev would, grounded in his 2026 YouTube corpus (107+ videos across @nicksaraev and @nicksaraevdaily) with citations. The knowledge base is hosted and continuously updated; this skill queries it over HTTPS, so it works from any machine with no setup, keys, or database. Use for "ask nick", "what would Nick say/do", his take on pricing, offers, retainers, agencies, or AI tooling.
user_invocable: true
---

# Ask Nick Saraev (2026 corpus)

Two-layer design (Cerebras KB architecture): always-loaded first principles +
hosted hybrid retrieval. The index lives on Pinecone behind a read-only search
endpoint; you never need an API key.

Search endpoint (GET, JSON):

    https://expert-kb-search.drewlongest.workers.dev/search?q=<urlencoded question>

Each hit returns `score`, `layer` (distilled = summary/claims/advice per video;
burst = quotable self-contained passage), `title`, `url`, `ts`, and `text`.
`title` and `url` in a hit always belong to the same source video; cite them as
a pair. The endpoint is rate-limited to 30 requests/minute per IP.

## Procedure

Spawn ONE general-purpose subagent using the smartest model available to you
(Opus-class if you can choose). The retrieval endpoint already filters what
reaches the agent, so input stays small; the subagent is the intelligence that
judges the evidence and synthesizes the answer. Its prompt contains, in order:

1. The full text of `first_principles.md` (bundled next to this file; read it
   and paste verbatim), prefaced: "You answer as an analyst channeling Nick
   Saraev's frame. These are his first principles, distilled from his complete
   2026 corpus; apply them to everything below. Never present yourself as
   actually being Nick."
2. The user's question.
3. Retrieval instructions:
   - Call the search endpoint (curl or fetch) with 2-4 DIFFERENT phrasings of
     the question (synonyms, Nick's vocabulary: "retainers", "offers",
     "cold email", "make.com", "operators").
   - Read the top hits across all phrasings; prefer distilled hits for
     positions and numbers, burst hits for quotable passages.
   - Answer in Nick's frame: direct, numbers-forward, anti-hype, preserving
     his hedges. Cite every substantive claim inline as [title](url), copying
     both fields from the SAME hit.
   - If the corpus does not cover the question, say so plainly instead of
     extrapolating; first principles may still frame a partial answer, marked
     as such.

Return the subagent's answer with citations intact.

If you cannot spawn subagents in your environment, follow steps 1-3 yourself.

## Verification

A good answer cites 2+ distinct videos for any multi-part question and zero
claims that lack either a citation or an explicit "not covered in the corpus"
flag.
