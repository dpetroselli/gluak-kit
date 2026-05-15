# CLAUDE.md — <PROJECT NAME>

This file is auto-loaded by Claude Code at session start. It is the **portable knowledge
base** for this repo — it travels with the git repo, so any session opened from any clone
has full context.

Detailed knowledge lives in **`context/`**. This file is the index + the always-true
essentials. **When a task touches a specific area, read the relevant `context/` file
before acting.**

---

## What this is

<!-- TODO: 2-4 lines — what the project is, who it is for, what this repo holds. -->

---

## Always-true essentials

<!-- TODO: the handful of facts and rules that are true every session and must never be
     violated. Keep this short — detail goes in context/ files. -->

---

## Critical operational rules

<!-- TODO: anything dangerous or irreversible to be careful about. Delete this section if
     there is nothing of the sort. -->

---

## Current state (update this section as things change)

<!-- TODO: a living snapshot of where the project is right now. -->

---

## `context/` index — read the relevant file when the task calls for it

| File | When to read it |
|---|---|
| `context/overview.md` | Always relevant — what the project is |

---

## Knowledge-capture & push workflow

- **Claude updates `context/` autonomously.** As soon as durable knowledge is learned or a
  state changes during a session, Claude edits the relevant `context/` file (or
  `CLAUDE.md`) without being asked — this keeps important things from being lost between
  sessions. After doing so, Claude tells the user in **one line, once**, what was noted
  and where — then drops it.
- **These updates stay as uncommitted working-tree changes.** Claude never commits or
  pushes autonomously. Do NOT repeat "it's in queue / I'm not pushing" each turn, and do
  NOT acknowledge git-reminder hooks every turn — the user knows the workflow.
- **Commit & push are manual — on the user's explicit instruction only.** When the user
  says "push", commit *everything pending* (context updates + any code changes) and push.
