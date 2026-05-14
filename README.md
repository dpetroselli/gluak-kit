# gluak-claude-kit

Daniele Petroselli's personal Claude Code toolkit — a plugin marketplace of skills,
agents and helpers tuned to the way Gluak works. It grows over time as new tooling is
developed.

## Plugins

| Plugin | What it does |
|---|---|
| `repo-memory` | Portable, repo-versioned project knowledge base — a root `CLAUDE.md` (index + essentials + workflow) plus a `context/` directory of detailed topic files that travels with the git repo and is restored on any checkout. |

## Install

In any Claude Code session, on any machine:

```
/plugin marketplace add dpetroselli/gluak-claude-kit
/plugin install repo-memory@gluak-claude-kit
```

Then invoke a plugin's skill with its namespaced name — e.g. `/repo-memory:repo-memory`.

## Update

```
/plugin marketplace update gluak-claude-kit
```

A plugin updates only when its `version` (in its `plugin.json`) is bumped.

## Layout

```
gluak-claude-kit/
├── .claude-plugin/
│   └── marketplace.json          # the marketplace catalog
└── plugins/
    └── repo-memory/
        ├── .claude-plugin/
        │   └── plugin.json       # the plugin manifest
        └── skills/
            └── repo-memory/
                ├── SKILL.md
                ├── INSTALL.md
                └── templates/
```

Skills are auto-discovered from each plugin's `skills/` directory — no need to declare
them in `plugin.json`.
