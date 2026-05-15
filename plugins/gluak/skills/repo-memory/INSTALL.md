# Installing repo-memory on other machines

This skill has two homes by design:

1. **Versioned in this repo** at `.claude/skills/repo-memory/` — it travels with every
   checkout, so the skill is available wherever this repo is cloned.
2. **Optionally global** at `~/.claude/skills/repo-memory/` — available in *every* project
   on a machine, not just this repo.

## Install globally on a new machine

From the root of a checkout of this repo:

```sh
mkdir -p ~/.claude/skills
cp -r .claude/skills/repo-memory ~/.claude/skills/
```

After that, `/repo-memory` is available in any project on that machine.

## Keep the two copies in sync

The repo copy is canonical. After editing the skill in the repo, refresh the global copy:

```sh
cp -r .claude/skills/repo-memory ~/.claude/skills/
```

Or symlink instead of copying, so the global skill always tracks the repo copy:

```sh
ln -s "$(pwd)/.claude/skills/repo-memory" ~/.claude/skills/repo-memory
```
