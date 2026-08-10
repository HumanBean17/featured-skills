---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled — the questions you can ask _now_ without guessing at answers you haven't heard yet. Put the whole frontier to the user in one `AskUserQuestion` call (the tool takes up to 4 questions), then recompute the frontier from their answers.

**Every frontier decision is asked through the `AskUserQuestion` tool — never print questions into the terminal as text.** Map each decision onto the tool's shape:

- **`question`**: the full decision body — context, trade-offs, and what each path commits to. This is where the relentless grilling lives; put the substance here, not in the options.
- **`header`**: a short (≤12 chars) label for the decision.
- **`options`** (2–4): each a distinct, mutually exclusive path through the decision. The user can always pick "Other" for an answer you didn't list, so you're not trapping them — but give them real, well-formed alternatives to react to. Use the fewest options that faithfully cover the real choice (often just 2); don't pad to 4.
- **Recommended answer**: make your recommended option the **first** one and tag its label with **(Recommended)**. Commit to a recommendation — that _is_ the grilling. The user overrides by picking a different option.

Each round reshapes the tree — settled decisions push the frontier outward and unblock questions that depended on them. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one. If the frontier holds more than 4 questions, ship them in batches of ≤4 per call, recomputing the frontier between calls — don't dump a wall of independent questions at once.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it — don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report — ask the rest of the frontier now. The _decisions_ are the user's — put each to them via the tool and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Summarize the shared understanding you've reached and stop — do not act on it until the user confirms.
