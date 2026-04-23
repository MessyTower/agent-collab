# agent-collab

**Multi-agent collaboration for GitHub repos.**

Drop `.agent-collab/` into any repo. Any AI agent that clones it automatically discovers the coordination system and can work alongside other agents without conflicts.

## How It Works

```
your-repo/
├── .agent-collab/          ← Self-contained coordination tool
│   ├── collab              ← CLI (python, zero deps)
│   ├── manifest.json       ← Who's working on what
│   ├── claims.json         ← Who owns which files
│   └── log.jsonl           ← Activity feed
├── src/
├── tests/
├── README.md               ← Auto-injected with discovery hook
└── CLAUDE.md               ← Auto-injected with discovery hook
```

**Any agent cloning this repo sees the instructions automatically** — in README.md, CLAUDE.md, .cursorrules, AGENTS.md, or any other file they check.

## Quick Start (Repo Owner)

```bash
# Copy .agent-collab/ into your repo
cp -r .agent-collab/ /path/to/your-repo/.agent-collab/

# Initialize (injects discovery hooks into README, CLAUDE.md, etc.)
cd /path/to/your-repo
python3 .agent-collab/collab init
```

That's it. Every agent that clones the repo will see the coordination instructions.

## Quick Start (AI Agent)

When you clone a repo and see `.agent-collab/`:

```bash
# 1. See what's happening
.agent-collab/collab status

# 2. Register yourself
.agent-collab/collab register --name "your-name" --task "what you're doing"

# 3. Claim files before editing
.agent-collab/collab claim src/your-area/ --reason "your task"

# 4. Work on your files...

# 5. Release when done
.agent-collab/collab release src/your-area/

# 6. Signal completion
.agent-collab/collab ready
```

## Commands

| Command | Description |
|---------|-------------|
| `init` | Set up coordination, inject discovery hooks |
| `register` | Register your agent with a task |
| `unregister` | Remove your agent |
| `claim` | Claim files/dirs for exclusive editing |
| `release` | Release claims |
| `status` | See all agents, claims, activity |
| `check <path>` | Check if a path is safe to edit |
| `conflicts` | Find overlapping claims |
| `ready` | Signal your work is done |
| `integrate` | Check if everyone's ready to merge |
| `log` | Write/read activity feed |
| `hook-install` | Install git pre-commit hook |

## Auto-Discovery

On `init`, coordination instructions are injected into:

- `README.md` (created if missing)
- `CLAUDE.md`
- `.cursorrules`
- `AGENTS.md`
- `.github/copilot-instructions.md`
- `CONTRIBUTING.md`
- `INSTRUCTIONS.md`

Any file with the `<!-- agent-collab:managed -->` marker stays in sync.

## Rules

1. **Claim before edit** — `collab claim <files>` before touching anything
2. **Never edit claimed files** — If someone else claimed it, coordinate
3. **Release when done** — `collab release` frees files for others
4. **Signal completion** — `collab ready` tells others you're done
5. **Check status** — `collab status` at the start of every session

## GitHub Actions

Copy `github-actions-check.yml` to `.github/workflows/` for CI enforcement.

## License

MIT
