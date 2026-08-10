---
tracker: beads
description: Wayfinding on the beads (bd) tracker — how each wayfinder act becomes a bd command.
---

# Beads (`bd`)

Beads is a dependency-graph issue tracker built for agents, so it already speaks wayfinder's language: issues with labels, native blocking, an atomic claim, a ready frontier, and comments. The map is a labelled **epic**; tickets are its **children**; the frontier is `bd ready` scoped to the map. Native blocking is what draws the frontier — `bd dep tree` — so no body convention is needed for it.

In everything the human reads, refer to an issue by its **title**; the `bd-…` id rides inside the name, never stands in for it.

## Wayfinding operations

**The map** — one issue, labelled `wayfinder:map`, typed `epic`. Its body — the **description** field, not notes/design/acceptance — holds the sections from SKILL.md: Destination, Notes, Decisions so far, Not yet specified, Out of scope.

- Create: `bd create --type epic -l wayfinder:map --title "<destination, as a title>" --body-file map.md`. Don't pass `--validate` — the map body is freeform.
- The `wayfinder:map` label is the identity; the `epic` type unlocks `bd children` and `bd epic status`. Both outlive any id.
- Load the low-res view: `bd show <map>` for the body, `bd children <map>` for the ticket roster (open and closed).
- Edit the body in place: `bd edit <map>` (opens `$EDITOR` on the description) or `bd update <map> --body-file map.md`. Appending a Decisions-so-far line is an edit, not a note.

**Tickets** — child issues of the map, typed by label.

- Create: `bd create --parent <map> -l wayfinder:<research|prototype|grilling|task> --title "<question, as a title>" --body-file ticket.md`. Pass `--silent` when you need just the id for wiring.
- The `wayfinder:<type>` label is the ticket type. It is orthogonal to beads' own `-t` (default `task`); set `-t decision` only when a ticket is pure decide-and-lock, and even then the label still carries the wayfinder type.
- Identity is the `bd-…` id. Zoom with `bd show <ticket>` (`--long`, or `--include-comments` under `--json`).

**Blocking (native)** — a ticket is unblocked when every blocker is closed.

- Wire: `bd dep add <blocked-ticket> <blocker-ticket>`. Create tickets first, then wire in a second pass — ids must exist before they can reference each other. For a big chart pass, bulk-wire with `bd dep add --file deps.jsonl` (`{"from":"<blocked>","to":"<blocker>"}` per line).
- See it drawn: `bd dep tree <map>` (`--format mermaid` for a flowchart). This is why blocking stays native — the tracker draws the frontier for the human, no map convention needed.

**Frontier** — the open, unblocked, unclaimed children.

- `bd ready --parent <map>`. `bd ready` already excludes in_progress (claimed), blocked, and deferred work, so scoped to the map's descendants this *is* the frontier; take the first in priority order. Add `--explain` to see why a ticket is or isn't ready, `-u` for unassigned only.

**Claim** — before any work.

- `bd update <ticket> --claim` (atomic: assignee + in_progress; idempotent if already yours), or claim the first frontier ticket in one step with `bd ready --parent <map> --claim`. The claim *is* the assignment; concurrent sessions skip a claimed ticket.

**Resolve** — the answer is recorded, not part of the body.

- Post the answer: `bd comment <ticket> "<answer>"`.
- Close with the gist as the reason: `bd close <ticket> --reason "<one-line gist>"` (`--suggest-next` names newly-unblocked work).
- Index on the map: append one line to the body's **Decisions so far** via `bd edit <map>` — the ticket's title (id inside) and the gist. The decision lives in the closed ticket; the map only points at it.
- Link assets (a prototype branch, a research dump), don't paste: a `bd comment` with the pointer, or `bd update <ticket> --external-ref "<url>"` when there's one canonical ref. Use `bd dep relate <ticket> <other>` for see-also knowledge-graph links.

**Graduate fog / rule out of scope.**

- Graduate: when a resolution sharpens fog into a question you can now state precisely, create the child ticket(s) and clear that patch from the body's **Not yet specified** (`bd edit <map>`).
- Out of scope: `bd close <ticket> --reason "out of scope: <why>"` and leave one line in the body's **Out of scope**. It does not enter Decisions so far.

**Notes & memory.**

- The body's **Notes** block carries this effort's standing preferences — per-map, loaded with the map. For axioms every session in this repo should hold regardless of map, use `bd remember "<insight>"`; `bd prime` injects memories at session start and survives compaction. Effort Notes stay in the map; project memory goes in `bd remember`.

**Concurrent sessions.** Beads is built for multi-agent: `--claim` is atomic and Dolt merges at cell level, so expect other sessions to be creating and closing tickets alongside you. Across machines, `bd dolt pull` on entry and `bd dolt push` on exit keep the graph shared.
