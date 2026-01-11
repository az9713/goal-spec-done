# Glossary & FAQ - Get Shit Done (GSD)

## Table of Contents

1. [Glossary of Terms](#glossary-of-terms)
2. [Frequently Asked Questions](#frequently-asked-questions)
3. [Common Error Messages](#common-error-messages)
4. [Keyboard Shortcuts](#keyboard-shortcuts)

---

## Glossary of Terms

### A

**Action**
The specific work to be done in a task. Written in imperative form ("Create", "Add", "Modify").

**Agent / Subagent**
A separate Claude instance spawned to execute work. Used for autonomous plan execution to maintain fresh context.

**Atomic Commit**
A single Git commit containing one logical change. GSD creates atomic commits for each task.

**Auto Task**
A task with `type="auto"` that Claude executes without human intervention.

### B

**Blocker**
An issue that prevents work from continuing. Must be resolved before proceeding.

### C

**Checkpoint**
A point in execution where Claude pauses for human input. Types: `human-verify`, `decision`, `human-action`.

**Claude Code**
Anthropic's command-line interface for Claude AI. GSD runs inside Claude Code.

**Command**
A slash command like `/gsd:plan-phase`. Entry point for GSD functionality.

**Commit**
A saved change in Git. GSD creates commits after each task and after plan completion.

**Context**
The files and information Claude reads before executing a task. Includes @references in PLAN.md.

**Context Window**
The amount of text Claude can process at once. GSD keeps plans small to avoid exhausting it.

### D

**Decision Checkpoint**
A checkpoint where the user must choose between options.

**Deviation**
Work discovered during execution that wasn't in the plan. Handled by deviation rules.

**Deviation Rules**
Five rules governing how to handle unplanned work:
1. Auto-fix bugs
2. Auto-add missing critical functionality
3. Auto-fix blocking issues
4. Ask about architectural changes
5. Log non-critical enhancements

**Discovery**
Research phase before planning. Produces DISCOVERY.md with findings.

**Done Criteria**
Conditions that must be true for a task to be complete.

### E

**Execute**
Running a plan to completion. The `/gsd:execute-plan` command.

**Execution Context**
The environment and state when a plan runs. Includes prior summaries and decisions.

### F

**Fresh Context**
A new Claude session with no prior conversation. Subagents get fresh context.

**Frontmatter**
YAML metadata at the top of Markdown files, between `---` markers.

### G

**GSD**
Get Shit Done - this workflow framework.

**Git**
Version control system that tracks changes. GSD requires Git.

### H

**Human Action Checkpoint**
A checkpoint requiring the user to perform an action Claude cannot (rare).

**Human Verify Checkpoint**
A checkpoint where the user verifies Claude's work (common).

### I

**Issue**
A logged problem or enhancement for future work. Stored in ISSUES.md.

**ISS Number**
Issue identifier like ISS-001. Auto-incremented.

### M

**Milestone**
A major version or release. Groups multiple phases (e.g., "v1.0 MVP").

**Mode**
Execution style: `interactive` (asks for approval), `yolo` (auto-approves), `custom`.

### P

**Phase**
A chunk of work with a clear goal. Contains 1+ plans.

**Phase Directory**
Folder like `.planning/phases/01-foundation/` containing phase files.

**Plan**
A detailed set of tasks within a phase. The PLAN.md file IS the executable prompt.

**PLAN.md**
The plan file containing tasks for Claude to execute.

**Planning Directory**
The `.planning/` folder containing all project state.

**PROJECT.md**
Document describing project vision, requirements, and constraints.

### R

**Reference**
Guidance documents in `references/` that inform Claude's behavior.

**Research**
Deep investigation before planning. Produces RESEARCH.md.

**Resume**
Continuing work after a pause or interruption.

**ROADMAP.md**
Document listing all phases and their status.

### S

**Segment**
A portion of a plan between checkpoints. May be executed by subagent.

**Session**
A single Claude Code conversation. State persists within a session.

**Slash Command**
A command starting with `/gsd:`. User interface for GSD.

**STATE.md**
Document tracking current position, decisions, and continuity.

**Subagent**
See Agent.

**SUMMARY.md**
Document created after plan execution. Records accomplishments and decisions.

**Success Criteria**
Conditions that define successful completion of a command or plan.

### T

**Task**
A single piece of work within a plan. Has type, action, verification, and done criteria.

**TDD**
Test-Driven Development. Write tests first, then implementation.

**Template**
A file structure definition in `templates/`. Used to create consistent files.

**Todo**
A future task or idea. Logged via `/gsd:add-todo`.

### U

**UAT**
User Acceptance Testing. Manual verification that features work as expected.

**UAT Issues**
Bugs found during user testing. Stored in `{phase}-{plan}-ISSUES.md`.

### V

**Verification**
Checking that a task completed successfully. Can be automated or manual.

**Verify**
The verification step in a task definition.

### W

**Workflow**
Detailed execution logic in `workflows/`. Commands reference workflows.

---

## Frequently Asked Questions

### Getting Started

**Q: What is GSD and why should I use it?**

GSD is a project management framework for AI-assisted development. It helps you:
- Break complex projects into manageable phases
- Track progress with clear documentation
- Maintain quality with structured verification
- Never lose work with automatic Git commits

**Q: Do I need to know how to code to use GSD?**

Basic familiarity with the command line helps. GSD is designed to work with Claude, who handles most of the technical work. You guide the vision; Claude handles the implementation.

**Q: How much does GSD cost?**

GSD itself is free. You need a Claude Code subscription from Anthropic to use it.

**Q: Can I use GSD with any programming language?**

Yes. GSD is language-agnostic. It's a workflow framework that works with any technology Claude can work with.

### Workflow

**Q: How long should a phase take?**

It depends on complexity:
- Simple phases: 15-30 minutes
- Medium phases: 30-60 minutes
- Complex phases: 1-2 hours

Keep phases focused. If a phase seems too big, split it.

**Q: How many tasks should a plan have?**

2-3 tasks per plan is ideal. This keeps context usage around 50% and maintains quality.

**Q: Can I skip a phase?**

Yes. Use `/gsd:remove-phase` to remove it from the roadmap. Or just don't execute it and move on.

**Q: What happens if Claude makes a mistake?**

Several options:
1. Tell Claude what's wrong and ask for a fix
2. Use Git to undo: `git revert HEAD`
3. Log it as an issue for later: `/gsd:add-todo "Fix X"`

**Q: Can I modify files manually while using GSD?**

Yes, but:
- Inform Claude of your changes
- Be careful not to conflict with active plans
- Manual changes aren't automatically committed

### Technical

**Q: Where is my project data stored?**

In the `.planning/` directory of your project:
- `PROJECT.md` - Project vision
- `ROADMAP.md` - All phases
- `STATE.md` - Current position
- `phases/` - Phase-specific files

**Q: Can I edit .planning files manually?**

Yes, they're just Markdown files. However:
- STATE.md is auto-updated; manual changes may be overwritten
- Keep formatting consistent with templates
- Be careful not to break YAML frontmatter

**Q: How does GSD use Git?**

GSD creates commits automatically:
- One commit per completed task
- One metadata commit per completed plan
- All changes are tracked for rollback

**Q: What if I don't want automatic commits?**

GSD requires Git for tracking. You can squash commits later if needed:
```bash
git rebase -i HEAD~N  # N = number of commits to squash
```

**Q: Can multiple people work on the same GSD project?**

GSD is designed for solo development, but you can:
- Coordinate through Git branches
- Each person works on different phases
- Merge through standard Git workflow

### Troubleshooting

**Q: GSD commands aren't recognized**

1. Verify installation: `ls ~/.claude/get-shit-done/`
2. Reinstall if needed: `npm install -g get-shit-done`
3. Restart Claude Code

**Q: Claude seems confused about the project**

1. Check STATE.md is accurate
2. Run `/gsd:progress` to reset context
3. Use `/gsd:resume-work` to reload state

**Q: My project is stuck in a bad state**

1. Check ROADMAP.md for current status
2. Check STATE.md for last activity
3. You can manually edit these files to fix state
4. Or start fresh with `/gsd:new-project` in a new directory

**Q: How do I recover from a failed execution?**

1. Check Git log: `git log --oneline`
2. Revert if needed: `git revert HEAD`
3. Resume: `/gsd:resume-task`

---

## Common Error Messages

### "Project not initialized"

**Meaning:** GSD can't find `.planning/` directory.

**Solution:**
```bash
/gsd:new-project
```

### "Git repository required"

**Meaning:** Current directory isn't a Git repository.

**Solution:**
```bash
git init
```

### "Phase not found"

**Meaning:** The specified phase doesn't exist in ROADMAP.md.

**Solution:**
1. Check available phases: `cat .planning/ROADMAP.md`
2. Use correct phase number

### "Plan already executed"

**Meaning:** SUMMARY.md exists for this plan.

**Solution:**
- Move to next plan
- Or delete SUMMARY.md to re-execute

### "Verification failed"

**Meaning:** Task verification didn't pass.

**Solution:**
1. Check what failed
2. Fix the issue manually
3. Re-run verification

### "Context limit exceeded"

**Meaning:** Too much content for Claude's context window.

**Solution:**
1. Use smaller plans (2-3 tasks)
2. Clear conversation: type `/clear`
3. Resume fresh: `/gsd:resume-work`

### "STATE.md missing"

**Meaning:** Project state file not found.

**Solution:**
1. Create manually from template
2. Or reinitialize: `/gsd:new-project`

---

## Keyboard Shortcuts

These work in Claude Code:

### Navigation

| Shortcut | Action |
|----------|--------|
| `Up Arrow` | Previous command |
| `Down Arrow` | Next command |
| `Ctrl + C` | Cancel current operation |
| `Ctrl + D` | Exit Claude Code |

### Text Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl + A` | Go to line start |
| `Ctrl + E` | Go to line end |
| `Ctrl + W` | Delete word backward |
| `Ctrl + U` | Delete line |
| `Ctrl + L` | Clear screen |

### Claude Code Specific

| Shortcut | Action |
|----------|--------|
| `Tab` | Accept suggestion |
| `/clear` | Clear conversation |
| `/help` | Show help |

---

## Quick Reference Card

### Most Used Commands

```
/gsd:new-project       Start new project
/gsd:progress         Where am I?
/gsd:plan-phase N     Plan phase N
/gsd:execute-plan     Run the plan
/gsd:verify-work      Test the work
/gsd:pause-work       Save and stop
/gsd:resume-work      Continue later
```

### File Locations

```
~/.claude/get-shit-done/     GSD installation
.planning/                    Project state
.planning/PROJECT.md          Project vision
.planning/ROADMAP.md          All phases
.planning/STATE.md            Current position
.planning/phases/             Phase files
```

### Checkpoint Responses

```
"approved"    Accept verification
"done"        Completed action
"option-id"   Select option
"describe"    Explain issue
```

### Deviation Rules

```
Rule 1: Bug → Fix immediately
Rule 2: Critical → Add immediately
Rule 3: Blocking → Fix to continue
Rule 4: Architectural → Ask first
Rule 5: Nice-to-have → Log for later
```
