---
name: ask-nick-saraev-lite
description: Answer a question the way Nick Saraev would, grounded in his Lite knowledge base (469 videos across @nicksaraev and @nicksaraevdaily, plus his newsletters, blog, free assets, and free Maker Zero course) with citations. The knowledge base is hosted and continuously updated; the answering agent queries it over HTTPS, so this works from any machine with no setup, keys, or database. Use for "ask nick", "what would Nick say/do", his take on pricing, offers, retainers, agencies, or AI tooling.
user_invocable: true
---

# Ask Nick Saraev (Lite knowledge base)

This skill is a thin dispatcher. All of the intelligence lives in the
`ask-nick-saraev-lite` subagent (installed at `~/.claude/agents/ask-nick-saraev-lite.md`),
whose own definition carries Nick's first principles, the epistemic rules
(answer only from retrieved knowledge-base context, never from training data),
and the retrieval procedure. Those load into the subagent's context on every
spawn; the parent never needs to read or paste them.

## Procedure

1. Spawn the `ask-nick-saraev-lite` subagent with the user's question as the
   entire prompt. Do not add instructions, context, or your own framing; the
   agent definition already contains everything it needs.
2. Return the subagent's answer with citations intact.

If the `ask-nick-saraev-lite` agent type is not available (not installed, or the
environment does not support custom agents), read
`~/.claude/agents/ask-nick-saraev-lite.md`, strip its frontmatter, and use the
body verbatim as the prompt preamble for a general-purpose subagent (smartest
model available) with the question appended. If neither file nor subagents
exist, follow that body's instructions yourself.

## Verification

A good answer cites 2+ distinct videos for any multi-part question and zero
claims that lack either a citation or an explicit "not covered in the corpus"
flag.
