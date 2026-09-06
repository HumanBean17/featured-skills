---
name: to-tickets-beads
description: Break a plan, spec, or the current conversation into a set of tracer-bullet tickets on the beads (bd) tracker, blocking edges wired as native bd dependencies. Beads-only — every publish operation is a bd command, no tracker to choose.
disable-model-invocation: true
---

# To Tickets

Break a plan, spec, or conversation into a set of **tickets** — tracer-bullet vertical slices, each declaring the tickets that **block** it — published to beads, the repo's `bd` issue tracker.

**Beads-only.** This edition is for repos that track work with beads (`bd`). Every publish operation below is a `bd` command — there's no tracker to choose. For the tracker-agnostic edition, see `to-tickets`.

## Process

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes a reference (a spec path, a beads issue id) as an argument, fetch it and read its full body and comments (`bd show <id> --long`; `--include-comments` under `--json`).

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code. Ticket titles and descriptions should use the project's domain glossary vocabulary, and respect ADRs in the area you're touching.

Look for opportunities to prefactor the code to make the implementation easier. "Make the change easy, then make the easy change."

### 3. Draft vertical slices

Break the work into **tracer bullet** tickets.

<vertical-slice-rules>

- Each slice cuts a narrow but COMPLETE path through every layer (schema, API, UI, tests) — vertical, NOT a horizontal slice of one layer
- A completed slice is demoable or verifiable on its own
- Each slice is sized to fit in a single fresh context window
- Any prefactoring should be done first

</vertical-slice-rules>

Give each ticket its **blocking edges** — the other tickets that must complete before it can start. A ticket with no blockers can start immediately.

**Wide refactors are the exception to vertical slicing.** A **wide refactor** is one mechanical change — rename a column, retype a shared symbol — whose **blast radius** fans across the whole codebase, so a single edit breaks thousands of call sites at once and no vertical slice can land green. Don't force it into a tracer bullet; sequence it as **expand–contract**. First expand: add the new form beside the old so nothing breaks. Then migrate the call sites over in batches sized by blast radius (per package, per directory), each batch its own ticket blocked by the expand, keeping CI green batch to batch because the old form still exists. Finally contract: delete the old form once no caller remains, in a ticket blocked by every migrate batch. When even the batches can't stay green alone, keep the sequence but let them share an integration branch that all block a final integrate-and-verify ticket — green is promised only there.

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each ticket, show:

- **Title**: short descriptive name
- **Blocked by**: which other tickets (if any) must complete first
- **What it delivers**: the end-to-end behaviour this ticket makes work

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the blocking edges correct — does each ticket only depend on tickets that genuinely gate it?
- Should any tickets be merged or split further?

Iterate until the user approves the breakdown.

### 5. Publish the tickets to beads

Create one beads issue per approved ticket, then wire the blocking edges in a **second pass** — ids must exist before they can reference each other.

- **Create** each ticket with `bd create --title "<title>" --body-file ticket.md` (no `--validate` — the body is freeform; `--silent` when you need just the id for wiring). Add `-l ready-for-agent` unless instructed otherwise — the tickets are agent-grabbable by construction. If the source was an existing beads issue, create the tickets as its children with `--parent <issue>`, and do NOT close or modify that parent.
- **Wire** each edge with `bd dep add <blocked-ticket> <blocker-ticket>`; for a big set, bulk-wire with `bd dep add --file deps.jsonl` (`{"from":"<blocked>","to":"<blocker>"}` per line).

Blocking is native, so the tracker draws the frontier itself: a ticket is unblocked when every ticket blocking it is closed — `bd ready` lists what can start now (`--parent <issue>` scopes it to the set), `bd dep tree` shows the whole shape. For a purely linear chain that means top to bottom.

Report the published set by **title** — the `bd-…` id and its `bd show` link ride inside the name, never stand in for it.

<ticket-template>

## What to build

The end-to-end behaviour this ticket makes work, from the user's perspective — not layer-by-layer implementation.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

</ticket-template>

Parent and blocking edges are native — `--parent` and `bd dep add` — so the body carries neither section.

Avoid specific file paths or code snippets — they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.
