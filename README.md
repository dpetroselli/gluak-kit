# gluak-kit

Daniele Petroselli's personal Claude Code marketplace — one plugin (`gluak`) that bundles
every Gluak skill: project bootstrap, portable repo memory, and any helpers added in the
future. Grows over time.

## What's inside

Single plugin: **`gluak`**. Skills shipped with it:

| Skill | What it does |
|---|---|
| `gluak-setup` | Bootstrap a new project the Gluak way — git init + Gluak `.gitignore`, scaffold the portable `CLAUDE.md` + `context/` knowledge base (via `repo-memory`), write the Gluak Bash-call convention into the project's `CLAUDE.md`, reduce permission prompts (via Anthropic's `fewer-permission-prompts`). Run once per new project. |
| `repo-memory` | Portable, repo-versioned project knowledge base — a root `CLAUDE.md` (index + essentials + workflow) plus a `context/` directory of detailed topic files that travels with the git repo and is restored on any checkout. |

## Install

In any Claude Code session, on any machine:

```
/plugin marketplace add dpetroselli/gluak-kit
/plugin install gluak@gluak-kit
```

One install, all skills available. Invoke a skill with its namespaced slash command —
`/gluak:gluak-setup`, `/gluak:repo-memory`, etc. — or just by describing the task
(skills auto-trigger from their `description`).

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
            ├── gluak-setup/
            │   └── SKILL.md
            └── repo-memory/
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
