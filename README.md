# gluak-kit

Daniele Petroselli's personal Claude Code marketplace — one plugin (`gluak`) that bundles
every Gluak skill: project bootstrap, portable repo memory, and any helpers added in the
future. Grows over time.

## Install

In any Claude Code session, on any machine:

```
/plugin marketplace add dpetroselli/gluak-kit
/plugin install gluak@gluak-kit
```

One install, all skills available. Invoke a skill with its namespaced slash command —
`/gluak:setup`, `/gluak:memory`, etc. — or just by describing the task (skills
auto-trigger from their `description`).

## What's inside

Single plugin: **`gluak`**. Skills shipped with it:

| Skill | What it does |
|---|---|
| `setup` | Bootstrap a new project the Gluak way — git init + Gluak `.gitignore`, scaffold the portable `CLAUDE.md` + `context/` knowledge base (via `memory`), write the Gluak Bash-call convention into the project's `CLAUDE.md`, reduce permission prompts (via Anthropic's `fewer-permission-prompts`). Run once per new project. |
| `memory` | Portable, repo-versioned project knowledge base — a root `CLAUDE.md` (index + essentials + workflow) plus a `context/` directory of detailed topic files. The whole knowledge base lives **in the git repo**, so a `git clone` / `checkout` / `pull` on any machine restores the full project memory. |

### `gluak:setup` — project bootstrap, in one shot

Runs Daniele's standard project setup runbook so every new project starts the same way:

1. **Git init + Gluak `.gitignore`** — `git init` if needed; merges Gluak-flavoured rules
   (`.claude/*` + `!.claude/skills/`, secrets, deps, build) into any existing
   `.gitignore` without overwriting.
2. **Scaffold portable repo memory** — invokes `gluak:memory` to create or integrate
   `CLAUDE.md` + `context/`.
3. **Bash-call convention** — appends the "single commands, not compound" project rule
   to `CLAUDE.md`. This is what makes the per-command allowlist actually silence
   permission prompts: the permission system matches the *full* command string, so
   compound `&&` / `;` / pipe commands never match per-command allowlist entries —
   granular Bash calls do.
4. **Reduce permission prompts** — invokes Anthropic's `fewer-permission-prompts` to
   build a per-project allowlist from past transcripts. Skipped on fresh projects with
   no transcripts to learn from.

Idempotent. Re-running `gluak:setup` on an already-set-up project only fills what's
missing. Never overwrites, never commits.

### `gluak:memory` — make project memory travel with the repo

What it solves: by default, Claude Code's persistent memory lives **machine-locally**
(at `~/.claude/projects/.../memory/`). That means as soon as you clone the project on
another machine — or pull on a CI runner, or open it on a teammate's laptop — Claude
starts with **zero context**: no decisions, no conventions, no history of what you've
tried, no list of "don't do X." Every fresh checkout is a cold start.

`gluak:memory` flips that around. The knowledge base is **two files in the git repo**:

- **`CLAUDE.md`** at the repo root — auto-loaded every session. Holds the always-true
  essentials: who/what the project is, critical operational rules, current state, plus
  an index of the `context/` files. Kept short on purpose, because every session reads
  it in full.
- **`context/<topic>.md`** — one file per topic area (architecture, deploy, conventions,
  domain facts, …). Read on demand when a task touches that area, so depth doesn't bloat
  every session's context window.

Because both live in the git repo, **`git clone` / `git checkout` / `git pull` on any
machine restores the full project memory** — the codebase and the memory ship together.
No "I forgot to set up memory on this laptop", no out-of-sync memory across machines, no
lost decisions when a teammate joins.

`gluak:memory` runs in three modes automatically:

- **Scaffold** (no `CLAUDE.md`, no `context/`) — creates both from templates with
  `<!-- TODO -->` placeholders for what only you can answer.
- **Adopt** (`CLAUDE.md` exists, no `context/`) — preserves your existing `CLAUDE.md`,
  splits the genuinely deep material into `context/` files, and appends the index +
  capture/push workflow without rewriting what you wrote.
- **Maintenance** (both exist) — captures durable knowledge into `context/` as it
  emerges in a session, keeps `CLAUDE.md` lean, and updates the index when new topic
  files are added.

The skill never commits or pushes — that's always the user's call.

## Update

```
/plugin marketplace update gluak-kit
```

The plugin updates only when its `version` (in `plugin.json`) is bumped.

## Layout

```
gluak-kit/
├── .claude-plugin/
│   └── marketplace.json              # the marketplace catalog
└── plugins/
    └── gluak/
        ├── .claude-plugin/
        │   └── plugin.json           # the plugin manifest
        └── skills/
            ├── setup/
            │   └── SKILL.md
            └── memory/
                ├── SKILL.md
                ├── INSTALL.md
                └── templates/
```

Skills are auto-discovered from `plugins/gluak/skills/` — adding a new skill is just a
new directory with a `SKILL.md` inside. No manifest edits needed.

## Adding a new skill

1. Create `plugins/gluak/skills/<your-skill>/SKILL.md` with frontmatter (`name`,
   `description`) followed by the runbook.
2. Bump `version` in `plugins/gluak/.claude-plugin/plugin.json` and in
   `.claude-plugin/marketplace.json`.
3. Commit, push. Users get it on the next `/plugin marketplace update gluak-kit`.
