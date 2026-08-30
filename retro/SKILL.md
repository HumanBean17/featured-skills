---
name: retro
description: End-of-session reflection — surface the session's friction as one-line proposals appended to the reflections backlog.
argument-hint: "[focus area]"
disable-model-invocation: true
---

You just finished (or are about to finish) a session. Your job is to surface the **friction** — things that were not obvious, time sinks, wrong assumptions, detours — and turn each into a one-line proposal a human can later triage into the harness. The human watched the session happen; the friction is the part they couldn't see from outside, so a summary of what you did is the wrong output. This runs **whether or not the session was a "success."** You lived this session, so reflect on it directly and unconditionally. This skill is local-only: no network, nothing leaves the machine. If the user passed an argument naming a focus area, weight your evidence there.

## 0. If this session has no prior turns

If you were just started and there's nothing to reflect on yet, say so in one line ("Session just started — nothing to reflect on.") and stop there.

## 1. Ground yourself in evidence (do this first)

Before writing anything, scan **this** session for concrete friction. Look for:

- **Files** you opened repeatedly; searched for and couldn't find; or assumed existed but didn't (a class that "should be here" but isn't part of the project; a generated file you treated as source).
- **Commands / tools** you retried, that failed unexpectedly, or whose output surprised you (an API that didn't behave as named; a flag that meant the opposite; a tool that needs a prerequisite you didn't have).
- **Wrong assumptions** you had to correct (a dependency that needed installing; a convention opposite to what you assumed; a test that only passes offline / in a specific order).
- **Detours** — paths you went down and backed out of.

If the user pastes other friction notes or tool output from this session, treat it as *additional* evidence — but **your lived experience outranks it when they disagree.** If pasted material already captured the same friction, fold it into your entry rather than writing it twice.

## 2. Check the backlog for recurrence (before writing)

Recurrence is the signal worth acting on, and it changes what you write. Grep the backlog for the key nouns of the friction you found in step 1:

```
grep -i -E "keyword1|keyword2|keyword3" docs/_reflections/log.md
```

(If the backlog doesn't exist yet, there is no recurrence to find — skip ahead.)

- **Prior match** — this friction has come up before. Say so explicitly: "This is the Nth time `<theme>` has come up — strong candidate to promote out of the backlog into the harness." Carry the count into the entry itself (step 3), because the conversation is lost and the file is not.
- **No prior match** — say "no recurrence" and move on. A single hit can still be noise; only a repeated theme earns promotion.

## 3. Write 1–3 proposals — seeds, not decrees

Emit **at most 3** proposals (fewer is fine). Each proposal is **one line**, and must be:

- **Specific to this session** — name the file, command, or assumption. If you cannot name a concrete file, command, or assumption, **drop it.**
- **Low-commitment** — a seed for a human, not an instruction to paste. Use a shape like:
  - "It was not obvious that `<concrete fact>`; would have helped to know `<this>` up front."
  - "Spent a while on `<X>` until I realized `<Y>`."
  - "Assumed `<Z>`; actually `<W>`."
- **Tagged** — end the line with `(→ <kind>)` where `<kind>` is exactly one of: `CLAUDE.md note`, `permission`, `command`, `doc`, `refactor`, `none`.

**Ban generic lessons.** The seed shapes above can be filled with platitudes, so don't let them be — "communicate more" / "plan carefully" / "read the docs" carry no information:

- Bad: "It was not obvious that tests need care; would have helped to know the order." *(no file, no command — generic)*
- Good: "Assumed the suite is order-independent; actually `auth.test.ts` only passes when run after `db.test.ts`. (→ CLAUDE.md note)"

## 4. Append to the backlog — and ONLY the backlog

**Your only file mutation is appending to `docs/_reflections/log.md`.** Every other file stays untouched — CLAUDE.md, `settings.json`, commands, all of it. Deciding what becomes a harness change is the human's job; your output is raw material, not the change.

Append a new section in this exact format (`<area>` from the fixed vocabulary below, so recurrence stays matchable across sessions):

```
## <ISO date> · <one-line session goal> · <success | partial | fail | clean>
- **<area>** — <one-line proposal ending in "(→ <kind>)">
```

`<area>` is one of: `setup`, `tooling`, `docs`, `test`, `build`, `other`. A recurring proposal carries its occurrence count in the line, e.g. `- **test** — \`chart.test.ts\` needs \`date-fns.test.ts\` first; not obvious solo. 2nd time. (→ CLAUDE.md note)`.

If the file does not exist, create a **minimal stub** first — a `# Reflections backlog` title, a one-line purpose, and the entry-format block above — then append.

## If the session was genuinely clean

Record it: append a `· clean` entry with no proposals ("Nothing notable — clean session."). A clean session is a valid outcome and keeps the backlog honest about which sessions happened. **Do not manufacture friction to justify the run.**
