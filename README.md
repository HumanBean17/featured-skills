# featured-skills

Curated agentic skills.

## Principle

Hardened: every skill is standalone, not part of any framework. Copy a folder, get a working skill — no shared dependencies, no orchestration stack, nothing else to install.

## Skills

### Mine

- **brief-me** — guided walkthrough of a document you need to understand: contract, law, spec, paper.
- **difit-giving-review** — run a code review and deliver it as anchored threads in a launched [difit](https://github.com/yoshiko-pg/difit) session; owns the delivery format, not the reviewing.
- **difit-receiving-review** — receive code-review comments from a running [difit](https://github.com/yoshiko-pg/difit) server, answer them in the difit threads, act on them; in a `difit --ladder` session each fix lands inside its rung (`fixup!` + autosquash, tree-proven).
- **git-review-ladder** — re-cut a branch's net diff into a few commits for rung-by-rung human review: one per intent with its tests and docs, on top of one proven prep commit for behavior-preserving noise; rewrites your own branch in place, builds a side branch for someone else's.
- **hold** — park the agent: it acknowledges, inventories the conversation's active skills, and does nothing until an explicit go-phrase releases it.
- **review-loop** — review a branch rung by rung to the end: cuts it into a review ladder once, opens one `difit --ladder` session, and answers each Submit with a difit-receiving-review pass whose fixes land inside their rungs, until Finish. Composes git-review-ladder and difit-receiving-review (both needed) and a difit with `--ladder` ([fork](https://github.com/HumanBean17/difit)).
- **retro** — end-of-session reflection; surfaces the session's friction as one-line proposals in a reflections backlog.
- **self-verify** — final stage of any task: restate the consumer's test as checkable criteria, close each with executed evidence before claiming done.

### From [mattpocock/skills](https://github.com/mattpocock/skills), as-is

- **handoff** — compact the current conversation into a handoff document for another agent to pick up.
- **prototype** — build a throwaway prototype to answer a design question.
- **to-spec** — turn the current conversation into a spec, published to the project issue tracker.
- **to-tickets** — break a plan, spec, or conversation into tracer-bullet tickets with declared blocking edges.
- **wayfinder** — plan work too big for one session as a map of decision tickets, resolved one at a time.

### From [mattpocock/skills](https://github.com/mattpocock/skills), modified

- **grill-me** — relentless interview to sharpen a plan or design; modified to always ask via the `AskUserQuestion` tool.
- **to-tickets-beads** — to-tickets reworked to use [beads](https://github.com/gastownhall/beads) (`bd`) only; blocking edges wired as native `bd` dependencies.
- **wayfinder-beads** — wayfinder reworked for [beads](https://github.com/gastownhall/beads); decision tickets live on the `bd` tracker, every operation is a `bd` command.
