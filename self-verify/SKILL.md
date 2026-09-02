---
name: self-verify
description: Self-verification as the native final stage of any task — before claiming work done, restate the consumer's test as checkable criteria and close each with executed evidence, fixing gaps before reporting. Use whenever about to complete, deliver, or hand over any piece of work, however small — emails, document edits, code changes, calculations, answers to questions.
---

# Self-verify

Work is done when it survives the **consumer's test** — the check the person receiving it would actually run — not when the part you looked at seems handled. Agents stop early in a characteristic way; call it **local completion**: asked to rename an enum throughout a spec, an agent renamed the first section, saw the ask satisfied, and stopped — every later section still carried the old name. Asked to verify, it found and fixed them all within a minute. The work was never hard; the stop was early.

Self-verification is a stage, not a character trait. Before calling any deliverable done, run this protocol. It costs a moment; the alternative is shipping the section-1-only rename.

## 1. Restate the consumer's test

Say what would make the consumer reject this work — both halves:

- **The stated asks**, each one, including small ones buried mid-sentence.
- **What they would check without saying**: the recipient's name spelled right, the tone fitting the audience, the calculation answering the question actually asked, every occurrence updated — the exhaustive reading of "rename", "remove", "update".

The literal ask is a lower bar than the real one: "rename X in the doc" means *no X remains anywhere*, not "X renamed where I happened to look". Phrase each criterion as a checkable condition ("a search for the old name returns nothing") — the next step closes each one with evidence.

## 2. Close each criterion with evidence

Produce evidence per criterion, strongest form first:

- **Executed** — run the search, substitute the answer back into the original equation, run the tests, re-read the actual file. Close what you can this way.
- **Inspected** — read the finished artifact against the criterion, in full. The move for artifacts with no test suite: an email is verified by re-reading the text that will be sent against the ask, not by remembering what you meant to write.
- **Recalled** — "I did that earlier." Memory of doing is not an outcome; treat it as a pointer to evidence, valid only under the freshness rule below.

**Freshness governs reuse.** Evidence already produced this session counts for exactly as long as the thing it verifies stays untouched, and expires the moment that thing changes: a green suite is dead the edit after it ran. Cite what is fresh, rerun what is stale — and only what is stale: after a change, re-verify what the change could touch, not the whole world. Freshness is what keeps this protocol cheap enough to run on everything, including the two-line task.

## 3. The verdict carries its evidence

Steps 1 and 2 are legwork, not deliverable — the checklist lives in your reading of the work, and the reader sees only the verdict. A rendered checklist turns a moment of care into ceremony, and ceremony is the reason verification gets skipped. State done as a claim with its evidence attached, in a line or two:

- "Renamed throughout — a search for the old name over the doc returns nothing."
- "Fixed — `node slugify.test.js`: 5/5 pass."
- "Ready to send — re-read against your asks: Dana named, Friday deadline stated, tone kept casual."

A criterion that fails is fixed before it is reported: fix, re-verify per freshness, then name what was caught as part of the claim. A gap you cannot fix — missing information, two criteria in conflict — gets surfaced as exactly that, with the blocked criterion named.

A done-claim without evidence is a guess about work you are holding. Verify, then speak.
