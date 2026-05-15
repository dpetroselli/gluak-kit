# Installing the `memory` skill

The standard install is via the `gluak-kit` marketplace:

```
/plugin marketplace add dpetroselli/gluak-kit
/plugin install gluak@gluak-kit
```

After that, `/gluak:memory` is available in every project on that machine.

## In-repo vendoring (optional, advanced)

If you want the skill to travel **with a specific project's git repo** — so any clone
of that project on any machine has the skill available without installing the
marketplace — vendor the skill into the project's `.claude/skills/` directory:

```sh
mkdir -p .claude/skills
cp -r ~/.claude/plugins/marketplaces/gluak-kit/plugins/gluak/skills/memory \
      .claude/skills/
```

Then make sure the project's `.gitignore` keeps committed skills versioned:

```gitignore
.claude/*
!.claude/skills/
```

Commit `.claude/skills/memory/` to the repo. Now any clone of that repo gets the skill
automatically. (This is the same pattern `gluak:setup` enforces in step 1 of its
runbook.)
