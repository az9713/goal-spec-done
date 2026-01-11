# Command Reference - Get Shit Done (GSD)

Complete reference for all 24 GSD commands with detailed usage instructions.

---

## Quick Reference

| Command | Purpose | Arguments |
|---------|---------|-----------|
| `/gsd:new-project` | Initialize new project | None |
| `/gsd:create-roadmap` | Create phase roadmap | None |
| `/gsd:map-codebase` | Analyze existing code | None |
| `/gsd:plan-phase` | Create phase plan | `[phase-number]` |
| `/gsd:execute-plan` | Execute current plan | `[plan-path]` (optional) |
| `/gsd:verify-work` | User acceptance testing | `[phase-or-plan]` (optional) |
| `/gsd:progress` | Show current status | None |
| `/gsd:pause-work` | Save state for later | None |
| `/gsd:resume-work` | Continue after break | None |
| `/gsd:resume-task` | Resume interrupted task | `[agent-id]` (optional) |
| `/gsd:add-phase` | Add new phase | `[description]` |
| `/gsd:insert-phase` | Insert urgent phase | `[after-phase]` `[description]` |
| `/gsd:remove-phase` | Remove a phase | `[phase-number]` |
| `/gsd:discuss-phase` | Define phase vision | `[phase-number]` |
| `/gsd:research-phase` | Deep ecosystem research | `[phase-number]` |
| `/gsd:list-phase-assumptions` | Show Claude's assumptions | `[phase-number]` |
| `/gsd:add-todo` | Add future task | `[description]` |
| `/gsd:check-todos` | Review pending todos | `[area]` (optional) |
| `/gsd:consider-issues` | Review deferred issues | None |
| `/gsd:plan-fix` | Create fix plan | `[plan-path]` (optional) |
| `/gsd:new-milestone` | Start new milestone | `[name]` |
| `/gsd:discuss-milestone` | Define milestone goals | None |
| `/gsd:complete-milestone` | Archive finished milestone | None |
| `/gsd:help` | Show command help | None |

---

## Detailed Command Reference

### Project Setup Commands

---

#### `/gsd:new-project`

**Purpose:** Initialize a new GSD project with proper structure.

**Arguments:** None

**What it does:**
1. Asks questions about your project vision
2. Creates `.planning/` directory structure
3. Creates `PROJECT.md` with your requirements
4. Sets up initial state tracking

**Prerequisites:**
- Git repository initialized (`git init`)
- Empty or new project directory

**Output files:**
- `.planning/PROJECT.md`
- `.planning/config.json`

**Example:**
```
/gsd:new-project

Claude: What's your project name?
You: My Todo App

Claude: Brief description?
You: A simple task management app with categories

Claude: What depth? (quick/standard/comprehensive)
You: standard
```

---

#### `/gsd:create-roadmap`

**Purpose:** Create the phase roadmap for your project.

**Arguments:** None

**What it does:**
1. Reads PROJECT.md for requirements
2. Breaks project into logical phases
3. Creates ROADMAP.md with all phases
4. Initializes STATE.md for tracking

**Prerequisites:**
- PROJECT.md exists
- Project vision is clear

**Output files:**
- `.planning/ROADMAP.md`
- `.planning/STATE.md`

**Example:**
```
/gsd:create-roadmap

Claude creates:
- Phase 1: Project Foundation
- Phase 2: Core Features
- Phase 3: User Interface
- Phase 4: Polish & Testing
```

---

#### `/gsd:map-codebase`

**Purpose:** Analyze an existing codebase before adding GSD.

**Arguments:** None

**What it does:**
1. Spawns parallel agents to analyze code
2. Documents technology stack
3. Maps architecture patterns
4. Records conventions and testing

**Prerequisites:**
- Existing code in repository

**Output files:**
- `.planning/codebase/STACK.md`
- `.planning/codebase/ARCHITECTURE.md`
- `.planning/codebase/STRUCTURE.md`
- `.planning/codebase/CONVENTIONS.md`
- `.planning/codebase/TESTING.md`
- `.planning/codebase/INTEGRATIONS.md`
- `.planning/codebase/CONCERNS.md`

**Example:**
```
/gsd:map-codebase

Claude: Analyzing codebase...
- Detected: React, TypeScript, Node.js
- Architecture: Component-based frontend
- 47 source files analyzed
- Documentation created in .planning/codebase/
```

---

### Phase Management Commands

---

#### `/gsd:plan-phase`

**Purpose:** Create detailed execution plan for a phase.

**Arguments:**
- `[phase-number]` - Which phase to plan (e.g., `1`, `2`, `3`)

**What it does:**
1. Reads phase details from ROADMAP.md
2. Breaks phase into 2-3 task plans
3. Creates PLAN.md files with tasks
4. Sets up verification steps

**Prerequisites:**
- ROADMAP.md exists
- Previous phases complete (if dependencies exist)

**Output files:**
- `.planning/phases/XX-name/XX-01-PLAN.md`
- (Additional plans if phase is large)

**Example:**
```
/gsd:plan-phase 2

Claude: Planning Phase 2: User Authentication

Created 2 plans:
- 02-01-PLAN.md: Login system (3 tasks)
- 02-02-PLAN.md: Registration (2 tasks)

Next: /gsd:execute-plan
```

---

#### `/gsd:execute-plan`

**Purpose:** Execute a plan, completing all tasks.

**Arguments:**
- `[plan-path]` (optional) - Specific plan file to execute

**What it does:**
1. Identifies next plan to execute
2. Executes each task with verification
3. Handles checkpoints for human input
4. Creates atomic commits per task
5. Creates SUMMARY.md when complete

**Prerequisites:**
- PLAN.md file exists
- Previous plans in phase complete

**Output files:**
- `.planning/phases/XX-name/XX-YY-SUMMARY.md`
- Modified source files (as defined in plan)

**Example:**
```
/gsd:execute-plan

Claude: Executing 02-01-PLAN.md

Task 1: Create login endpoint... done
Task 2: Add form validation... done
Task 3: Connect authentication...

CHECKPOINT: Please verify login form works
Visit: http://localhost:3000/login
Type "approved" when verified

You: approved

Task 3: Complete
Summary: .planning/phases/02-auth/02-01-SUMMARY.md
```

---

#### `/gsd:add-phase`

**Purpose:** Add a new phase at the end of the roadmap.

**Arguments:**
- `[description]` - What the phase should accomplish

**What it does:**
1. Reads current ROADMAP.md
2. Adds new phase at the end
3. Updates phase count

**Prerequisites:**
- ROADMAP.md exists

**Example:**
```
/gsd:add-phase "Add dark mode theme support"

Claude: Added Phase 5: Dark Mode Theme
- Updated ROADMAP.md
- Ready for planning when you reach it
```

---

#### `/gsd:insert-phase`

**Purpose:** Insert an urgent phase between existing phases.

**Arguments:**
- `[after-phase]` - The phase number to insert after
- `[description]` - What the urgent work is

**What it does:**
1. Creates decimal phase (e.g., 2.1)
2. Inserts between specified phases
3. Updates ROADMAP.md

**Prerequisites:**
- Target phase exists and is complete

**Example:**
```
/gsd:insert-phase 2 "Critical security fix for auth"

Claude: Created Phase 2.1: Critical Security Fix
- Inserted after Phase 2, before Phase 3
- Decimal numbering preserves existing phase numbers
```

---

#### `/gsd:remove-phase`

**Purpose:** Remove a phase from the roadmap.

**Arguments:**
- `[phase-number]` - Which phase to remove

**What it does:**
1. Removes phase from ROADMAP.md
2. Renumbers subsequent phases
3. Cleans up phase directory

**Prerequisites:**
- Phase must not be complete
- Phase must exist

**Example:**
```
/gsd:remove-phase 4

Claude: Removed Phase 4: Polish
- Phase 5 renamed to Phase 4
- ROADMAP.md updated
```

---

#### `/gsd:discuss-phase`

**Purpose:** Gather context and vision for a phase before planning.

**Arguments:**
- `[phase-number]` - Which phase to discuss

**What it does:**
1. Asks questions about phase goals
2. Captures your vision and preferences
3. Creates CONTEXT.md for the phase

**Prerequisites:**
- ROADMAP.md exists
- Phase exists

**Output files:**
- `.planning/phases/XX-name/XX-CONTEXT.md`

**Example:**
```
/gsd:discuss-phase 3

Claude: Let's discuss Phase 3: User Interface

What's the overall visual style?
You: Modern, minimal, dark theme preferred

What's the most important screen?
You: The dashboard - users see it most
```

---

#### `/gsd:research-phase`

**Purpose:** Deep ecosystem research for complex phases.

**Arguments:**
- `[phase-number]` - Which phase to research

**What it does:**
1. Researches relevant libraries and tools
2. Identifies best practices
3. Documents patterns to follow
4. Warns about common pitfalls

**Prerequisites:**
- Phase involves new technology
- Phase is complex/unfamiliar domain

**Output files:**
- `.planning/phases/XX-name/XX-RESEARCH.md`

**Example:**
```
/gsd:research-phase 3

Claude: Researching Phase 3: 3D Visualization

Researched:
- Three.js ecosystem
- React Three Fiber patterns
- Performance best practices
- Common pitfalls to avoid

Created: .planning/phases/03-3d/03-RESEARCH.md
```

---

#### `/gsd:list-phase-assumptions`

**Purpose:** See what Claude assumed during planning.

**Arguments:**
- `[phase-number]` - Which phase to check

**What it does:**
1. Lists assumptions Claude made
2. Shows where assumptions came from
3. Lets you correct misunderstandings

**Prerequisites:**
- Phase has been planned

**Example:**
```
/gsd:list-phase-assumptions 2

Claude: Assumptions for Phase 2:

1. Using JWT for authentication (from PROJECT.md)
2. PostgreSQL database (from constraints)
3. Email/password login only (assumed - no OAuth mentioned)

Correct any assumptions?
```

---

### Progress Commands

---

#### `/gsd:progress`

**Purpose:** Show current project status.

**Arguments:** None

**What it does:**
1. Reads STATE.md
2. Calculates completion percentage
3. Shows current position
4. Suggests next steps

**Example:**
```
/gsd:progress

Project: My Todo App
═══════════════════════════════

Phase: 2 of 4 (User Authentication)
Plan: 1 of 2 in current phase
Status: In progress

Progress: ████████░░░░░░░░░░░░ 40%

Last Activity: Completed 01-03-PLAN.md

Next Up:
- Complete 02-01-PLAN.md
- Then: 02-02-PLAN.md
```

---

#### `/gsd:verify-work`

**Purpose:** Guide user acceptance testing of completed work.

**Arguments:**
- `[phase-or-plan]` (optional) - What to test (defaults to most recent)

**What it does:**
1. Reads SUMMARY.md to understand what was built
2. Generates test checklist
3. Guides you through each test
4. Logs any issues found

**Prerequisites:**
- Work has been completed
- SUMMARY.md exists

**Output files:**
- `.planning/phases/XX-name/XX-YY-ISSUES.md` (if issues found)

**Example:**
```
/gsd:verify-work 2

Claude: Testing Phase 2: User Authentication

Test 1: Login form displays correctly
Did this work as expected?
- Pass / Fail / Partial / Skip

You: Pass

Test 2: Invalid credentials show error
...
```

---

### Workflow Commands

---

#### `/gsd:pause-work`

**Purpose:** Save state when you need to stop working.

**Arguments:** None

**What it does:**
1. Updates STATE.md with current position
2. Creates continuation file
3. Saves context for resumption

**Output files:**
- `.planning/.continue-here.md`

**Example:**
```
/gsd:pause-work

Claude: Work paused.

Current position saved:
- Phase 2, Plan 1, Task 2
- Resume with /gsd:resume-work

Take a break!
```

---

#### `/gsd:resume-work`

**Purpose:** Continue after a break or new session.

**Arguments:** None

**What it does:**
1. Reads STATE.md and continuation file
2. Restores context
3. Shows where you left off
4. Offers to continue

**Example:**
```
/gsd:resume-work

Claude: Welcome back!

You were working on:
- Phase 2: User Authentication
- Plan 1: Login system
- Task 2: Add form validation

Continue from here?
```

---

#### `/gsd:resume-task`

**Purpose:** Resume an interrupted subagent execution.

**Arguments:**
- `[agent-id]` (optional) - Specific agent to resume

**What it does:**
1. Checks for interrupted agents
2. Resumes execution from where it stopped
3. Continues normal flow

**Example:**
```
/gsd:resume-task

Claude: Found interrupted agent: abc123

Resuming Task 2: Form validation...
```

---

### Issue Management Commands

---

#### `/gsd:add-todo`

**Purpose:** Capture an idea or task for later.

**Arguments:**
- `[description]` - What to remember

**What it does:**
1. Adds todo to pending list
2. Tags with current phase context
3. Saves for later review

**Output files:**
- Updates `.planning/todos/pending/`

**Example:**
```
/gsd:add-todo "Add password strength indicator"

Claude: Todo added.
Review later with /gsd:check-todos
```

---

#### `/gsd:check-todos`

**Purpose:** Review pending todos.

**Arguments:**
- `[area]` (optional) - Filter by area

**What it does:**
1. Lists all pending todos
2. Shows context for each
3. Lets you select one to work on

**Example:**
```
/gsd:check-todos

Pending Todos:
1. Add password strength indicator (from Phase 2)
2. Implement dark mode toggle (from Phase 3)
3. Add export to CSV feature (from Phase 4)

Select one to work on, or "skip"
```

---

#### `/gsd:consider-issues`

**Purpose:** Review deferred issues for prioritization.

**Arguments:** None

**What it does:**
1. Reads ISSUES.md
2. Shows all open issues
3. Helps prioritize and close resolved issues

**Example:**
```
/gsd:consider-issues

Open Issues:
- ISS-001: Add rate limiting (P2 - security)
- ISS-002: Optimize database queries (P3 - performance)
- ISS-003: Improve error messages (P4 - UX)

Close resolved or schedule for work?
```

---

#### `/gsd:plan-fix`

**Purpose:** Create a plan to fix issues from testing.

**Arguments:**
- `[plan-path]` (optional) - Which plan's issues to fix

**What it does:**
1. Reads UAT issues file
2. Creates targeted fix plan
3. Focuses only on fixing reported issues

**Prerequisites:**
- `/gsd:verify-work` has been run
- Issues were logged

**Output files:**
- `.planning/phases/XX-name/XX-YY-FIX.md`

**Example:**
```
/gsd:plan-fix

Claude: Creating fix plan for 02-01-ISSUES.md

Found 2 issues:
- UAT-001: Login form doesn't show errors (Major)
- UAT-002: Password field too narrow (Minor)

Created: 02-01-FIX.md
Execute with /gsd:execute-plan
```

---

### Milestone Commands

---

#### `/gsd:new-milestone`

**Purpose:** Start a new milestone after completing one.

**Arguments:**
- `[name]` - Name for the new milestone

**What it does:**
1. Creates new milestone section in ROADMAP.md
2. Prepares for new phase planning
3. Updates STATE.md

**Prerequisites:**
- Previous milestone complete

**Example:**
```
/gsd:new-milestone "v2.0 - Advanced Features"

Claude: Created milestone: v2.0 - Advanced Features
Ready to add phases with /gsd:add-phase
```

---

#### `/gsd:discuss-milestone`

**Purpose:** Define goals for the next milestone.

**Arguments:** None

**What it does:**
1. Asks about milestone goals
2. Captures priorities and scope
3. Creates context for planning

**Output files:**
- `.planning/milestones/XX-CONTEXT.md`

**Example:**
```
/gsd:discuss-milestone

Claude: What's the goal for the next milestone?
You: Add team collaboration features

Claude: What's the most important feature?
You: Shared task lists between team members
```

---

#### `/gsd:complete-milestone`

**Purpose:** Archive a completed milestone and prepare for next.

**Arguments:** None

**What it does:**
1. Marks all phases complete
2. Creates milestone archive
3. Updates ROADMAP.md
4. Prepares for new milestone

**Prerequisites:**
- All phases in milestone complete

**Output files:**
- `.planning/milestones/XX-ARCHIVE.md`

**Example:**
```
/gsd:complete-milestone

Claude: Completing v1.0 MVP

Archived:
- 4 phases complete
- 12 plans executed
- Summary saved to archive

Ready for /gsd:new-milestone
```

---

### Help Command

---

#### `/gsd:help`

**Purpose:** Show help and command list.

**Arguments:** None

**What it does:**
1. Lists all available commands
2. Shows brief descriptions
3. Points to detailed documentation

**Example:**
```
/gsd:help

GET SHIT DONE - Command Reference

Project Setup:
  /gsd:new-project      Initialize new project
  /gsd:create-roadmap   Create phase roadmap
  /gsd:map-codebase     Analyze existing code

Phase Management:
  /gsd:plan-phase [N]   Create phase plan
  /gsd:execute-plan     Execute current plan
  ...

Full documentation: docs/COMMAND_REFERENCE.md
```

---

## Command Combinations

### Starting a New Project

```
/gsd:new-project
/gsd:create-roadmap
/gsd:plan-phase 1
/gsd:execute-plan
```

### Working on Existing Project

```
/gsd:map-codebase
/gsd:new-project
/gsd:create-roadmap
/gsd:plan-phase 1
```

### Daily Workflow

```
/gsd:resume-work
/gsd:progress
/gsd:execute-plan
/gsd:verify-work
/gsd:pause-work
```

### Handling Issues

```
/gsd:verify-work
# Mark issues during testing
/gsd:plan-fix
/gsd:execute-plan
/gsd:verify-work
```

### Completing Work

```
/gsd:execute-plan  # Complete final plan
/gsd:verify-work   # Final testing
/gsd:complete-milestone
/gsd:new-milestone "v2.0"
```
