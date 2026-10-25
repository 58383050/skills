---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Word each question so "yes" accepts your recommended answer.

## Keep the interview findable

Number questions continuously across rounds, starting at Q1. Never reset or reuse an ID within an interview. Give each question a short topic label drawn from the user's vocabulary. Reuse topic groups as the interview grows; group related questions within a round, but never move a dependent question into the frontier just to keep a group together.

Before sending a round that reaches **Q10**, create a **grilling results document**. This counts individual questions, not rounds: a first round of twelve questions already crosses the threshold. Include Q1 onward, including unanswered questions in the upcoming round. Backfill earlier answers from available sources; mark anything unrecoverable as missing rather than reconstructing it from a recommendation. Q10 starts recordkeeping, not a question limit.

Use a new session-specific `results.md` under `.scratch/grilling/` in the user's working directory. Without a working directory, use a new session-specific folder in the OS temporary directory. Show the absolute path once and keep using it for this interview. Never overwrite another interview or write into an installed skill directory. If file writes are unavailable or prohibited, say so and provide the same Markdown document in the conversation; do not claim it was saved or will survive lost context. Shorter interviews do not need a file unless the user requests one.

Keep the document small enough to consult, with these sections:

- **Original brief:** requirements, constraints, non-goals, and corrections, preserving exact wording where a paraphrase would change the decision.
- **Topic index:** each group's label, question IDs, and settled/open counts. Counts describe recorded state, not an estimate of how many questions remain.
- **Questions and results:** for each ID, retain the question, topic, prerequisites, agent recommendation, user's answer, and status (`unanswered`, `open`, `settled`, or `reopened`). Keep recommendations separate from confirmed answers. Preserve important qualifications and supporting quotes or source references; never invent source locations.
- **Next frontier:** unresolved decisions and their dependencies, plus the next unused question ID.

After each user reply, update answers, corrections, statuses, and the index before asking more questions. Before each new round, consult the original brief and relevant topic's existing questions, including those worded differently. Reuse settled answers and leave already-asked unanswered questions under their existing IDs rather than inventing new ones. Reopen a decision only when the user changes it or new evidence invalidates a prerequisite: retain its ID and prior answer, record why it reopened, and recheck affected dependents. A new decision gets a new ID.

On resuming, read the document and its relevant sources before recomputing the frontier. Treat explicit user corrections as authoritative; a saved recommendation is not consent. If a necessary answer is missing or conflicts with its source, name that specific gap rather than silently filling it or restarting the whole interview. Refresh the document at the end and include its path in the final shared-understanding check. Recordkeeping is part of the interview, not permission to implement the outcome.

## Continue the interview

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
