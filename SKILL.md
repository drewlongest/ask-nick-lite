---
name: ask-nick-2026
description: Answer a question the way Nick Saraev would, grounded in his 2026 YouTube corpus (107+ videos across @nicksaraev and @nicksaraevdaily) with citations. The knowledge base is hosted and continuously updated; the answering agent queries it over HTTPS, so this works from any machine with no setup, keys, or database. Use for "ask nick", "what would Nick say/do", his take on pricing, offers, retainers, agencies, or AI tooling.
user_invocable: true
---

# Ask Nick Saraev (2026 corpus)

This skill is a thin dispatcher. All of the intelligence lives in the
`nick-saraev-2026` subagent (installed at `~/.claude/agents/nick-saraev-2026.md`),
whose own definition carries Nick's first principles, the epistemic rules
(answer only from retrieved knowledge-base context, never from training data),
and the retrieval procedure. Those load into the subagent's context on every
spawn; the parent never needs to read or paste them.

## Procedure

1. Spawn the `nick-saraev-2026` subagent with the user's question as the
   entire prompt. Do not add instructions, context, or your own framing; the
   agent definition already contains everything it needs.
2. Return the subagent's answer with citations intact.

If the `nick-saraev-2026` agent type is not available (not installed, or the
environment does not support custom agents), read
`~/.claude/agents/nick-saraev-2026.md`, strip its frontmatter, and use the
body verbatim as the prompt preamble for a general-purpose subagent (smartest
model available) with the question appended. If neither file nor subagents
exist, follow that body's instructions yourself.

## Verification

A good answer cites 2+ distinct videos for any multi-part question and zero
claims that lack either a citation or an explicit "not covered in the corpus"
flag.
