---
name: gluak-setup
description: Bootstrap a new project the Gluak way — runs Daniele Petroselli's standard project setup runbook in one go. Initializes git (if needed) with a Gluak-flavoured `.gitignore`, scaffolds the portable `CLAUDE.md` + `context/` knowledge base via the `repo-memory` skill, and reduces permission prompts via the Anthropic `fewer-permission-prompts` skill. Use when starting a new project or repo, when the user says "set up this project", "bootstrap this repo", "gluak setup", "/gluak-setup", or when they want their standard Gluak project scaffolding applied.
---

# gluak-setup

Project bootstrap for Daniele's standard Gluak workflow. One invocation = the same
opinionated setup applied consistently to every new project.

## When this skill runs

Trigger when the user is starting a new project / repo and wants the standard scaffolding
applied — e.g. they ran `/gluak-setup`, said "bootstrap this repo", "set up this project
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

Invoke the `repo-memory` skill (same plugin). It detects scaffold / adopt / maintenance
mode automatically based on whether `CLAUDE.md` and `context/` already exist, and creates
or integrates them as needed.

### 3. Reduce permission prompts

Invoke the Anthropic `fewer-permission-prompts` skill. It scans recent transcripts for
read-only Bash and MCP tool calls that triggered permission prompts and adds a prioritized
allowlist to `.claude/settings.json`.

If there are no transcripts yet (fresh project), tell the user this step will be more
useful after a few sessions of real work, and **skip it** rather than producing an empty
allowlist.

### 4. Report

End with a one-line summary of what was done and what still needs the user's input
(typically: the `<!-- TODO -->` placeholders in `CLAUDE.md` from the `repo-memory`
scaffold, and any project-specific `context/` files to fill in).

## Conventions

- **Idempotent.** Re-running `gluak-setup` on an already-set-up project is safe: each
  step detects current state and does only what's missing.
- **Never overwrite.** Existing files are merged or left alone — never replaced.
- **Never commit.** This skill leaves everything as working-tree changes. Commit is
  always the user's call.
- **Be brief.** One line per step. No celebratory recap, no marketing. Daniele wants the
  signal, not the noise.

## Extending the runbook

Add new steps here as the standard Gluak setup grows. Keep ordering intentional —
foundational steps (git, gitignore) first, then knowledge base, then tooling
configuration.
