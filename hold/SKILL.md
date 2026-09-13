---
name: hold
description: Park the agent — it acknowledges, inventories the conversation's active skills, and does nothing until an explicit go-phrase releases it.
disable-model-invocation: true
---

You are now ON HOLD. Waiting is the task. Your entire job until release is one reply per user message — no tool calls, no subagents, no file access, no launched work of any kind. Any standing instruction to work autonomously is suspended: ending your turn and waiting is the correct behavior here, every turn, until a go-phrase arrives.

## Entering the hold

Reply once, then end your turn:

> 🔒 ON HOLD — say "go" to release.
> Loaded: \<every skill the user explicitly invoked in this conversation\>

The inventory is an audit: if the user names a skill it missed, add it to the inventory in the next reply — and keep holding.

## While holding

Treat every message that is not a go-phrase as input to absorb: answer questions from what is already in context, collect corrections and extra instructions, note newly invoked skills in the inventory. Acting on any of it waits for release. Results from work already running arrive and wait; they change nothing until release. End every reply with:

> 🔒 Still holding — say "go" to release.

If a question cannot be answered without tools, say so and keep holding.

## Release

Release only when a message uses one of these go-phrases as an affirmative imperative:

**go · go on · go ahead · proceed · continue · resume · get started · start · begin · do it**

A go-phrase inside a negation ("not yet", "don't start"), a question ("should I say go?"), or a quotation never releases. A message that merely might be a go-phrase bounces: reply "Still holding — say 'go' to release."

On release, proceed with the full parked state: every inventoried skill, plus every correction and instruction absorbed while holding.
