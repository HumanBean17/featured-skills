# briefing — design

Approved 2026-08-16 via grilling session.

## Problem

A reader receives a document their task requires. Its structure is foreign to them; reading it raw is slow, effortful, and error-prone. In human life the fix is a man who sends the doc, gets on a call, and narrates it — with the listener asking questions. `briefing` makes the agent that man.

## Core metaphor

The colleague who already read the document, on a call: spoken register, short turns, one idea at a time, questions welcome at every step.

## Decisions

1. **Trigger** — both: explicit `/briefing <path-or-URL>` and model-invoked via description. Description is pushy, covers "walk me through / explain / what am I signing / brief me on this doc", explicitly excludes one-shot summarize/translate/extract (no dialogue = different job).
2. **Voice** — conversational call register. Narrate in the reader's language regardless of doc language; key terms quoted in original wording with a gloss. Analogies allowed, flagged when they strain.
3. **Genre-blind** — no domain heuristics, no example patterns, anywhere in the body. The agent decides what matters and how to order it from reading the doc itself. Examples anchor to fake boundaries.
4. **Protocol** (the whole skill):
   - **Full read first** — entire doc read before any output; chunked if large; always. The map's honesty depends on it.
   - **Intake** — four questions (task, familiarity, depth, time) via AskUserQuestion when the tool is available, plain conversation otherwise; if waved off, state inferred defaults in one line and go.
   - **Message 1 = the map** — big picture (the general layer) + stops ordered general→particular (agent-designed, not the doc's outline) + declared skips, dependency-checked (anything kept stops silently rely on becomes part of a stop).
   - **Stops** — one concept per message, ≤300 words / one screen, essence→particulars, progress marker ("Stop 2 of 7"), gate as AskUserQuestion (continue / go deeper / wrap up; free-text answer = the reader's question) when available, spoken invitation otherwise. No anchors in the narrative — locations on demand; the anchor map lives in the recap only.
   - **Reader sovereignty** — questions answered now, never deferred ("another session, full re-read" is the failure this skill exists to prevent). Short question → short answer, resume. Deep question → immediate detour: consecutive ≤300-word chunks, each with its own gate.
   - **Re-plan, announced** — agent may revise the map when conversation reveals a misfit; shows revised map + what changed; reader can veto.
   - **Recap** — TLDR for the reader's task, map annotated covered/skipped, anchors of covered stops. The bridge back to working with the original.
5. **Non-goals** — no genre playbooks; no files written; no cross-session resume (recap is the exit); one primary doc (a second doc is context unless a second briefing is asked for); one-shot digest requests are answered directly, skill steps aside.
6. **Placement** — `featured-skills/briefing/` (SKILL.md + agents/openai.yaml, `allow_implicit_invocation: true`), installed copy to `~/.claude/skills/briefing/`.

## TLDR

`briefing` = the man on the call: full read → AskUserQuestion intake → message 1 with big picture + task-driven stop map + declared skips → one-concept ≤300-word stops with structured gates (no anchors in narrative) → questions answered immediately, deep dives as chunked detours → announced re-planning → TLDR recap with the anchor map back into the doc. Genre-blind by design: pure interaction protocol, zero domain examples.
