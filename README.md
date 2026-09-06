# featured-skills

Curated agentic skills.

## Principle

Hardened: every skill is standalone, not part of any framework. Copy a folder, get a working skill — no shared dependencies, no orchestration stack, nothing else to install.

## Skills

### Mine

- **brief-me** — guided walkthrough of a document you need to understand: contract, law, spec, paper.
- **difit-receiving-review** — receive code-review comments from a running [difit](https://github.com/yoshiko-pg/difit) server, answer them in the difit threads, act on them.
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
