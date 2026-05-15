---
name: setup
description: Bootstrap a new project the Gluak way — runs Daniele Petroselli's standard project setup runbook in one go. Initializes git (if needed) with a Gluak-flavoured `.gitignore`, scaffolds the portable `CLAUDE.md` + `context/` knowledge base via the `memory` skill, writes the Gluak Bash-call convention into the project's `CLAUDE.md`, and reduces permission prompts via the Anthropic `fewer-permission-prompts` skill. Use when starting a new project or repo, when the user says "set up this project", "bootstrap this repo", "gluak setup", "/gluak:setup", or when they want their standard Gluak project scaffolding applied.
---

# setup

Project bootstrap for Daniele's standard Gluak workflow. One invocation = the same
opinionated setup applied consistently to every new project.

## When this skill runs

Trigger when the user is starting a new project / repo and wants the standard scaffolding
applied — e.g. they ran `/gluak:setup`, said "bootstrap this repo", "set up this project
the usual way", or similar.

Confirm the target directory with the user before writing anything if it is not obviously
the cwd. Then execute the runbook below **in order**, reporting one short line after each
step.

## Runbook

### 1. Git init + Gluak `.gitignore`

- If the directory is **not** a git repo: `git init`.
- Ensure `.gitignore` at the repo root contains, at minimum:

  ```gitignore
  # Claude Code — machine-specific state out, versioned skills in
  .claude/*
  !.claude/skills/

  # Secrets
  *.token
  *.key
  .env
  .env.*

  # Deps & build
  node_modules/
  dist/
  build/
  .DS_Store
  ```

  If `.gitignore` already exists, **merge** these lines into it (don't overwrite);
  preserve any existing rules.
- Do **not** commit. Working-tree changes only.

### 2. Scaffold portable repo memory

Invoke the `memory` skill (same plugin). It detects scaffold / adopt / maintenance mode
automatically based on whether `CLAUDE.md` and `context/` already exist, and creates or
integrates them as needed.

### 3. Write the Gluak Bash-call convention into `CLAUDE.md`

This makes the "single calls, not compound commands" rule a project norm — auto-loaded
every session, so future Claude sessions on this project follow it without being asked.
The rule pairs with step 4 (the per-command allowlist): granular calls match the
allowlist, compound commands don't.

If the project's `CLAUDE.md` does **not** already contain the heading
`## Bash convention — single commands, not compound`, append this block verbatim at the
end of the file:

```markdown
## Bash convention — single commands, not compound

Emit many small Bash tool calls instead of chaining shell with `&&`, `;`, or pipes.
The permission system matches the **full command string** against the allowlist, so a
compound command never matches a per-command entry like `Bash(git status:*)` —
it triggers a permission prompt every time. Granular calls match the allowlist and
stay silent.

- Independent steps → separate Bash tool calls in the same response (the harness runs
  them in parallel).
- Steps with data flow → still separate calls when intermediate state can be routed via
  a temp file or a deterministic path.
- Chain with `&&` **only** when shell state must thread between steps in a way separate
  calls cannot reproduce (e.g. `cd <dir> && <relative-path-command>`). Prefer absolute
  paths and explicit env to remove even this case.
- Never use `;` (silently masks failures).
- Never pipe purely for output formatting that can be done in a second read or with a
  dedicated tool.

This rule was set by the `gluak:setup` skill.
```

If the heading already exists, **leave it alone** — idempotent. If it exists but has
clearly drifted, surface the drift to the user; do not silently rewrite it.

### 4. Reduce permission prompts

Invoke the Anthropic `fewer-permission-prompts` skill. It scans recent transcripts for
read-only Bash and MCP tool calls that triggered permission prompts and adds a prioritized
allowlist to `.claude/settings.json`.

If there are no transcripts yet (fresh project), tell the user this step will be more
useful after a few sessions of real work, and **skip it** rather than producing an empty
allowlist.

### 5. Report

End with a one-line summary of what was done and what still needs the user's input
(typically: the `<!-- TODO -->` placeholders in `CLAUDE.md` from the `memory` scaffold,
and any project-specific `context/` files to fill in).

## Conventions

- **Idempotent.** Re-running `setup` on an already-set-up project is safe: each step
  detects current state and does only what's missing.
- **Never overwrite.** Existing files are merged or left alone — never replaced.
- **Never commit.** This skill leaves everything as working-tree changes. Commit is
  always the user's call.
- **Be brief.** One line per step. No celebratory recap, no marketing. Daniele wants the
  signal, not the noise.

## Extending the runbook

Add new steps here as the standard Gluak setup grows. Keep ordering intentional —
foundational steps (git, gitignore) first, then knowledge base, then tooling
configuration.
