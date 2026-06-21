# kio-scaffold

A Claude Code **plugin marketplace** for Harness Engineering tooling.

> **Harness Engineering** = designing the *work environment itself* so that an AI
> can do good work on its own. This marketplace ships a plugin that sets that
> environment up for you automatically.

## Install

```bash
# 1) Add the marketplace
claude plugin marketplace add jsk4581/kio-scaffold

# 2) Install the plugin
claude plugin install harness@kio-scaffold
```

Once installed, the `/harness:scaffold` skill is ready to use.

## What's inside

### `harness` — Harness Engineering toolkit

| Skill | Invoke | What it does |
|-------|--------|--------------|
| scaffold | `/harness:scaffold` | Generates `CLAUDE.md` + `.claude/rules/` + a role-based folder structure in one pass. Interviews you for the stack and scope first, then fills everything in. On an existing project it also runs in a diagnose/simplify mode. |

#### What `/harness:scaffold` creates
```
your-project/
├── CLAUDE.md                       # Project map (kept short, ≤120 lines)
├── .claude/rules/
│   ├── project-structure.md        # Folder setup rules (always loaded)
│   ├── code-style.md               # Code style (always loaded)
│   ├── testing.md                  # Conditionally loaded by glob
│   └── security.md                 # Conditionally loaded by glob
├── docs/        # Human-owned docs (business truth)
├── .dev/        # AI-owned work records
│   └── collab/  # (optional) inter-agent collaboration — handoffs, board, claims
├── src/  tests/  out/
```

Core principles: User→Project→Folder scope inheritance, Progressive Disclosure,
double safety on boundaries, and "a good harness gets *simpler* over time."

### Inter-agent collaboration (optional)

When multiple agents or sessions work on one project — sequentially or
concurrently — they coordinate through files under `.dev/collab/`:

- **`handoff/`** — handoff notes for passing work to the next agent (what's done,
  what's left, gotchas, how to verify).
- **`DECISIONS.md`** — a lightweight shared decision log.
- **`BOARD.md`** — a live work board: who is doing what.
- **`claims/`** — per-task locks that prevent two agents from grabbing the same work.

**Git separation:** durable records (`handoff/`, `DECISIONS.md`) are committed for
sharing and review, while volatile live state (`BOARD.md`, `claims/`) is gitignored
to avoid merge conflicts.

**Worktree-based with symlinked shared state:** concurrent agents are isolated with
`git worktree` (one per agent) to avoid working-tree collisions. A worktree shares
commit history but *not* ignored files — so the gitignored `BOARD.md` and `claims/`
would fork per worktree and break coordination. The rule: symlink those volatile
files in each worktree to a single canonical copy in the primary checkout
(`ln -sfn`), so every agent sees the same live state. Tracked files
(`handoff/`, `DECISIONS.md`) need no symlink — git syncs them.

It's **opt-in** — single-agent projects skip it entirely (an empty coordination
folder is just noise).

## Repository layout

```
kio-scaffold/
├── .claude-plugin/marketplace.json     # Marketplace manifest
└── plugins/
    └── harness/
        ├── .claude-plugin/plugin.json  # Plugin manifest
        └── skills/
            └── scaffold/
                ├── SKILL.md
                ├── references/conventions.md
                └── assets/template/     # Templates used for generation
```

## Local development / testing

```bash
git clone https://github.com/jsk4581/kio-scaffold
claude plugin marketplace add ./kio-scaffold
claude plugin install harness@kio-scaffold
```

## Credits

Built on the structure / context / boundary principles from the Harness
Engineering course [Team Attention · Lee Ho-yeon](https://github.com/orgs/team-attention/)

## License

[MIT](LICENSE)
