# CLAUDE.md - Project Instructions for Claude Code

## Project Overview

**Get Shit Done (GSD)** is a Claude Code workflow framework that provides structured project management through slash commands. It helps developers plan, execute, and track software development work with AI assistance.

## Project Structure

```
get-shit-done_me/
├── bin/
│   └── install.js              # NPM post-install script
├── commands/
│   └── gsd/                    # 24 slash commands (*.md files)
├── get-shit-done/
│   ├── references/             # Reference documentation (9 files)
│   ├── templates/              # Project templates (14 files)
│   └── workflows/              # Core workflows (4 files)
├── .planning/                  # Example project state
│   ├── PROJECT.md             # Project context
│   ├── ROADMAP.md             # Phase roadmap
│   ├── STATE.md               # Current state
│   └── phases/                # Phase-specific files
├── package.json               # NPM package config
├── LICENSE                    # MIT License
└── README.md                  # Project readme
```

## Key Concepts

1. **Plans are Prompts**: PLAN.md files ARE executable prompts for Claude, not documentation
2. **Atomic Commits**: One commit per task, one metadata commit per plan
3. **Context Management**: ~50% context target per plan to maintain quality
4. **Deviation Rules**: 5 rules for handling unplanned work during execution
5. **Checkpoints**: Points requiring human verification or decisions

## Command Categories

### Project Setup
- `/gsd:new-project` - Initialize new project
- `/gsd:create-roadmap` - Create phase roadmap
- `/gsd:map-codebase` - Map existing codebase

### Phase Management
- `/gsd:plan-phase` - Create executable plans
- `/gsd:execute-plan` - Execute plans with commits
- `/gsd:add-phase`, `/gsd:insert-phase`, `/gsd:remove-phase`

### Workflow
- `/gsd:resume-work` - Resume after break
- `/gsd:verify-work` - User acceptance testing
- `/gsd:progress` - Show current status

## Development Guidelines

### When Modifying Commands
- Commands are Markdown files in `commands/gsd/`
- Each command has: purpose, arguments, process steps, success criteria
- Commands reference workflows in `get-shit-done/workflows/`

### When Modifying Workflows
- Workflows are in `get-shit-done/workflows/`
- Core workflows: plan-phase.md, execute-phase.md, verify-work.md
- Workflows use XML-like step tags for structure

### When Modifying Templates
- Templates are in `get-shit-done/templates/`
- Templates define structure for project artifacts
- Used by commands to create consistent files

## Testing Changes

1. Install locally: `npm link` in project root
2. Create test project: `mkdir test-project && cd test-project`
3. Run `/gsd:new-project` to test initialization
4. Walk through a complete phase cycle

## Common Patterns

### Reading Project State
```bash
cat .planning/STATE.md 2>/dev/null
cat .planning/ROADMAP.md
```

### Finding Current Phase
```bash
ls .planning/phases/
grep "In progress" .planning/ROADMAP.md
```

### Checking Plan Completion
```bash
ls .planning/phases/XX-name/*-PLAN.md | wc -l
ls .planning/phases/XX-name/*-SUMMARY.md | wc -l
```

## File Naming Conventions

- Phase directories: `XX-description/` (e.g., `01-foundation/`)
- Plan files: `{phase}-{plan}-PLAN.md` (e.g., `01-01-PLAN.md`)
- Summary files: `{phase}-{plan}-SUMMARY.md`
- Decimal phases for urgent work: `01.1-hotfix/`

## Commit Message Format

```
{type}({phase}-{plan}): {description}

Types: feat, fix, test, refactor, perf, docs, style, chore
```

## Important Constraints

- Plans should have 2-3 tasks maximum
- Target ~50% context usage per plan
- Always read STATE.md before operations
- Never skip checkpoint verification
- Document all deviations in SUMMARY.md
