# Architecture Guide - Get Shit Done (GSD)

## Overview

This document explains how GSD is designed and how all the pieces fit together. It's written for developers who want to understand the system deeply.

---

## Table of Contents

1. [System Architecture](#system-architecture)
2. [Component Deep Dive](#component-deep-dive)
3. [Data Flow](#data-flow)
4. [File Format Specifications](#file-format-specifications)
5. [State Management](#state-management)
6. [Execution Model](#execution-model)
7. [Extension Points](#extension-points)

---

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              USER INTERFACE                                  │
│                          (Claude Code Terminal)                              │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            COMMAND LAYER                                     │
│                                                                              │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐        │
│  │ new-project  │ │ plan-phase   │ │ execute-plan │ │ verify-work  │  ...   │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘        │
│                                                                              │
│  Location: commands/gsd/*.md                                                 │
│  Purpose: Entry points for user actions                                      │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            WORKFLOW LAYER                                    │
│                                                                              │
│  ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐             │
│  │  plan-phase.md   │ │ execute-phase.md │ │  verify-work.md  │             │
│  └──────────────────┘ └──────────────────┘ └──────────────────┘             │
│                                                                              │
│  Location: get-shit-done/workflows/*.md                                      │
│  Purpose: Detailed step-by-step execution logic                              │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                      ┌───────────────┼───────────────┐
                      ▼               ▼               ▼
┌────────────────────────┐ ┌──────────────────┐ ┌──────────────────────────┐
│     TEMPLATE LAYER     │ │  REFERENCE LAYER │ │     PROJECT STATE        │
│                        │ │                  │ │                          │
│ ┌────────────────────┐ │ │ ┌──────────────┐ │ │ ┌──────────────────────┐ │
│ │ project.md         │ │ │ │ checkpoints  │ │ │ │ .planning/           │ │
│ │ roadmap.md         │ │ │ │ principles   │ │ │ │   PROJECT.md         │ │
│ │ state.md           │ │ │ │ tdd.md       │ │ │ │   ROADMAP.md         │ │
│ │ phase-prompt.md    │ │ │ │ ...          │ │ │ │   STATE.md           │ │
│ │ summary.md         │ │ │ └──────────────┘ │ │ │   phases/            │ │
│ │ ...                │ │ │                  │ │ │     01-name/         │ │
│ └────────────────────┘ │ │ Location:        │ │ │       01-01-PLAN.md  │ │
│                        │ │ references/*.md  │ │ │       01-01-SUMMARY  │ │
│ Location:              │ │                  │ │ └──────────────────────┘ │
│ templates/*.md         │ │ Purpose:         │ │                          │
│                        │ │ Guidance docs    │ │ Location: .planning/     │
│ Purpose: File          │ │ for Claude       │ │ Purpose: Project data    │
│ structure definitions  │ │                  │ │                          │
└────────────────────────┘ └──────────────────┘ └──────────────────────────┘
```

### Component Relationships

```
Commands ─────────────► Workflows
    │                       │
    │                       │
    ▼                       ▼
Templates ◄────────────► References
    │                       │
    └──────────┬────────────┘
               │
               ▼
        Project State
       (.planning/*)
```

---

## Component Deep Dive

### 1. Command Layer

**Purpose**: Provide user-facing entry points for all GSD operations.

**Location**: `commands/gsd/*.md`

**Structure**:
```markdown
# Command name and brief description

<arguments>
$1 - First argument (required)
$2 - Second argument (optional)
</arguments>

<process>
1. Step one
2. Step two
3. Step three
</process>

<success_criteria>
- [ ] Criterion 1
- [ ] Criterion 2
</success_criteria>
```

**Key Commands**:

| Command | Type | Description |
|---------|------|-------------|
| `new-project` | Setup | Initialize GSD in a project |
| `plan-phase` | Planning | Create executable plans |
| `execute-plan` | Execution | Run plans with commits |
| `verify-work` | Verification | Guide user testing |
| `progress` | Status | Show current state |
| `pause-work` | Workflow | Save state for later |
| `resume-work` | Workflow | Restore saved state |

### 2. Workflow Layer

**Purpose**: Provide detailed execution logic that commands reference.

**Location**: `get-shit-done/workflows/*.md`

**Structure**:
```markdown
<purpose>
What this workflow accomplishes
</purpose>

<required_reading>
Files that must be read first
</required_reading>

<process>

<step name="step_name" priority="first|normal|last">
Detailed instructions for this step

<if mode="yolo">
What to do in yolo mode
</if>

<if mode="interactive">
What to do in interactive mode
</if>
</step>

</process>

<success_criteria>
- [ ] What must be true when complete
</success_criteria>
```

**Key Workflows**:

| Workflow | Purpose |
|----------|---------|
| `plan-phase.md` | Create PLAN.md files with proper structure |
| `execute-phase.md` | Execute plans with checkpoints and deviations |
| `verify-work.md` | Guide user acceptance testing |

### 3. Template Layer

**Purpose**: Define file structures for consistency.

**Location**: `get-shit-done/templates/*.md`

**Key Templates**:

| Template | Creates | Used By |
|----------|---------|---------|
| `project.md` | PROJECT.md | new-project |
| `roadmap.md` | ROADMAP.md | create-roadmap |
| `state.md` | STATE.md | new-project, execute-phase |
| `phase-prompt.md` | *-PLAN.md | plan-phase |
| `summary.md` | *-SUMMARY.md | execute-phase |
| `issues.md` | ISSUES.md | execute-phase |

### 4. Reference Layer

**Purpose**: Provide guidance documents for Claude.

**Location**: `get-shit-done/references/*.md`

**Key References**:

| Reference | Purpose |
|-----------|---------|
| `checkpoints.md` | How checkpoints work |
| `principles.md` | Core GSD principles |
| `plan-format.md` | PLAN.md structure rules |
| `scope-estimation.md` | Task sizing guidance |
| `tdd.md` | Test-driven development patterns |
| `git-integration.md` | Commit conventions |

### 5. Project State Layer

**Purpose**: Store all project-specific data.

**Location**: `.planning/` in user's project

**Structure**:
```
.planning/
├── PROJECT.md          # Project vision and requirements
├── ROADMAP.md          # All phases and their status
├── STATE.md            # Current position and context
├── ISSUES.md           # Deferred issues
├── config.json         # Project settings
└── phases/
    ├── 01-foundation/
    │   ├── 01-01-PLAN.md
    │   ├── 01-01-SUMMARY.md
    │   └── 01-CONTEXT.md (optional)
    ├── 02-features/
    │   ├── 02-01-PLAN.md
    │   └── ...
    └── ...
```

---

## Data Flow

### Command Execution Flow

```
User types /gsd:plan-phase 3
           │
           ▼
┌──────────────────────────────────────────────────────────────┐
│ 1. COMMAND PARSING                                           │
│    - Claude reads commands/gsd/plan-phase.md                 │
│    - Parses arguments ($1 = "3")                             │
└──────────────────────────────────────────────────────────────┘
           │
           ▼
┌──────────────────────────────────────────────────────────────┐
│ 2. WORKFLOW LOADING                                          │
│    - Command references workflows/plan-phase.md              │
│    - Claude reads and parses workflow                        │
└──────────────────────────────────────────────────────────────┘
           │
           ▼
┌──────────────────────────────────────────────────────────────┐
│ 3. CONTEXT LOADING                                           │
│    - Read STATE.md (current position)                        │
│    - Read ROADMAP.md (phase info)                            │
│    - Read PROJECT.md (project context)                       │
│    - Read relevant references                                │
└──────────────────────────────────────────────────────────────┘
           │
           ▼
┌──────────────────────────────────────────────────────────────┐
│ 4. STEP EXECUTION                                            │
│    - Execute each <step> in order                            │
│    - Handle conditionals (mode="yolo" etc.)                  │
│    - Interact with user at checkpoints                       │
└──────────────────────────────────────────────────────────────┘
           │
           ▼
┌──────────────────────────────────────────────────────────────┐
│ 5. OUTPUT GENERATION                                         │
│    - Create output files using templates                     │
│    - Update STATE.md                                         │
│    - Commit to Git                                           │
└──────────────────────────────────────────────────────────────┘
           │
           ▼
┌──────────────────────────────────────────────────────────────┐
│ 6. SUCCESS VERIFICATION                                      │
│    - Check all success_criteria                              │
│    - Report completion to user                               │
└──────────────────────────────────────────────────────────────┘
```

### Plan Execution Flow

```
/gsd:execute-plan
      │
      ▼
┌─────────────────┐
│ Load STATE.md   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Find next plan  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐    ┌─────────────────┐
│ Parse segments  │───►│ Check for       │
│ and checkpoints │    │ checkpoints     │
└────────┬────────┘    └────────┬────────┘
         │                      │
         ▼                      ▼
┌─────────────────┐    ┌─────────────────┐
│ Route to        │    │ No checkpoints: │
│ subagent or     │◄───│ Full subagent   │
│ main context    │    │                 │
└────────┬────────┘    └─────────────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│           FOR EACH TASK                  │
│  ┌───────────────────────────────────┐  │
│  │ 1. Execute task actions           │  │
│  │ 2. Apply deviation rules if needed│  │
│  │ 3. Run verification               │  │
│  │ 4. Commit changes                 │  │
│  │ 5. Handle checkpoints             │  │
│  └───────────────────────────────────┘  │
└────────────────┬────────────────────────┘
                 │
                 ▼
┌─────────────────┐
│ Create SUMMARY  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Update STATE    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Commit metadata │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Offer next step │
└─────────────────┘
```

---

## File Format Specifications

### PLAN.md Format

```markdown
---
phase: XX-name
plan: NN
type: standard|tdd
domain: optional-domain
---

<objective>
What this plan accomplishes in 2-3 sentences.
</objective>

<context>
@.planning/PROJECT.md
@.planning/STATE.md
@path/to/relevant/file.ts

**Key constraints:**
- Constraint from prior decisions
</context>

<tasks>

<task name="Task Name" type="auto|checkpoint:*" tdd="true|false">
**Files:** path/to/files
**Action:** What to do
**Verify:** How to verify
**Done:** Acceptance criteria
</task>

</tasks>

<verification>
How to verify the plan succeeded
</verification>

<success_criteria>
- [ ] Criterion 1
- [ ] Criterion 2
</success_criteria>

<output>
What files this plan creates/modifies
</output>

<commit>
type(phase-plan): description

- Change 1
- Change 2
</commit>
```

### SUMMARY.md Format

```markdown
---
phase: XX-name
plan: NN
subsystem: category
tags: [tech, keywords]
requires:
  - phase: prior-phase
    provides: what-it-built
provides:
  - what-this-built
affects: [future-phases]
tech-stack:
  added: [libraries]
  patterns: [patterns]
key-files:
  created: [files]
  modified: [files]
key-decisions:
  - "Decision 1"
patterns-established:
  - "Pattern 1"
issues-created: [ISS-XXX]
duration: Xmin
completed: YYYY-MM-DD
---

# Phase X Plan Y: Name Summary

**Substantive one-liner describing outcome**

## Performance

- **Duration:** X min
- **Started:** ISO timestamp
- **Completed:** ISO timestamp
- **Tasks:** N
- **Files modified:** N

## Accomplishments

- Accomplishment 1
- Accomplishment 2

## Task Commits

1. **Task 1** - `abc123f` (feat)
2. **Task 2** - `def456g` (fix)

## Files Created/Modified

- `path/file.ts` - What it does

## Decisions Made

Key decisions or "None - followed plan"

## Deviations from Plan

### Auto-fixed Issues

(If any)

### Deferred Enhancements

(If any)

## Issues Encountered

Problems and solutions or "None"

## Next Phase Readiness

What's ready, any blockers
```

### STATE.md Format

```markdown
# Project State

## Project Reference

See: .planning/PROJECT.md (updated DATE)

**Core value:** One-liner
**Current focus:** Phase name

## Current Position

Phase: X of Y (Phase name)
Plan: A of B in current phase
Status: Ready/In progress/Complete
Last activity: DATE - What happened

Progress: [░░░░░░░░░░] 0%

## Performance Metrics

**Velocity:**
- Total plans completed: N
- Average duration: X min
- Total execution time: X hours

## Accumulated Context

### Decisions

- [Phase X]: Decision

### Deferred Issues

- ISS-XXX: Description

### Blockers/Concerns

None or list

## Session Continuity

Last session: DATE TIME
Stopped at: Description
Resume file: Path or None
```

---

## State Management

### State Transitions

```
┌─────────────────────────────────────────────────────────────────┐
│                     PROJECT LIFECYCLE                            │
└─────────────────────────────────────────────────────────────────┘

     ┌──────────┐
     │ No State │  (Fresh directory)
     └────┬─────┘
          │ /gsd:new-project
          ▼
     ┌──────────┐
     │ Initialized │  PROJECT.md, ROADMAP.md, STATE.md exist
     └────┬─────┘
          │ /gsd:plan-phase 1
          ▼
     ┌──────────┐
     │ Planned  │  01-01-PLAN.md exists
     └────┬─────┘
          │ /gsd:execute-plan
          ▼
     ┌──────────┐
     │ Executing │  Tasks being processed
     └────┬─────┘
          │ All tasks complete
          ▼
     ┌──────────┐
     │ Complete │  01-01-SUMMARY.md exists
     └────┬─────┘
          │ More plans in phase?
          │
     ┌────┴────┐
     │Yes      │No
     ▼         ▼
   Plan      Phase
   Next      Complete
     │         │
     │         │ More phases?
     │    ┌────┴────┐
     │    │Yes      │No
     │    ▼         ▼
     │  Plan     Milestone
     │  Next     Complete
     │  Phase       │
     │              ▼
     │         /gsd:complete-milestone
     │              │
     └──────────────┘
```

### State File Updates

| Event | Files Updated |
|-------|---------------|
| Project created | PROJECT.md, ROADMAP.md, STATE.md created |
| Phase planned | PLAN.md created, ROADMAP.md updated |
| Plan executed | SUMMARY.md created, STATE.md updated, ROADMAP.md updated |
| Issue logged | ISSUES.md updated, STATE.md updated |
| Work paused | STATE.md updated, .continue-here.md created |
| Work resumed | STATE.md updated, .continue-here.md removed |

---

## Execution Model

### Task Execution

```
┌─────────────────────────────────────────────────────────────────┐
│                    TASK EXECUTION MODEL                          │
└─────────────────────────────────────────────────────────────────┘

For each task in PLAN.md:

1. CHECK TYPE
   ├── type="auto" ──────────────► Execute automatically
   └── type="checkpoint:*" ──────► Pause for human

2. IF AUTO:
   a. Read @context files
   b. Execute action steps
   c. IF deviation found:
      └── Apply deviation rules (1-5)
   d. Run verification
   e. IF verification passes:
      └── Commit changes
   f. ELSE:
      └── Stop, report failure

3. IF CHECKPOINT:
   a. Display checkpoint UI
   b. Wait for user input
   c. Process response
   d. Continue or handle issues
```

### Deviation Rules

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEVIATION DECISION TREE                       │
└─────────────────────────────────────────────────────────────────┘

Discovery during execution
          │
          ▼
    ┌─────────────┐
    │ Is it a bug │──Yes──► RULE 1: Fix immediately
    │ or error?   │
    └──────┬──────┘
           │ No
           ▼
    ┌─────────────┐
    │ Is it       │──Yes──► RULE 2: Add immediately
    │ critical?   │
    └──────┬──────┘
           │ No
           ▼
    ┌─────────────┐
    │ Does it     │──Yes──► RULE 3: Fix to unblock
    │ block work? │
    └──────┬──────┘
           │ No
           ▼
    ┌─────────────┐
    │ Is it       │──Yes──► RULE 4: Ask user first
    │ architectural?│
    └──────┬──────┘
           │ No
           ▼
    RULE 5: Log to ISSUES.md
```

### Subagent Routing

```
┌─────────────────────────────────────────────────────────────────┐
│                    SUBAGENT ROUTING LOGIC                        │
└─────────────────────────────────────────────────────────────────┘

Parse plan for checkpoints
          │
          ▼
    ┌─────────────┐
    │ Has any     │──No──► Pattern A: Full subagent
    │ checkpoints?│        (Fresh context, all tasks)
    └──────┬──────┘
           │ Yes
           ▼
    ┌─────────────────┐
    │ Are all         │──Yes──► Pattern B: Segmented
    │ verify-only?    │        (Subagent per segment)
    └──────┬──────────┘
           │ No
           ▼
    Pattern C: Main context
    (Decision affects following tasks)
```

---

## Extension Points

### Adding New Commands

1. Create `commands/gsd/new-command.md`
2. Follow command structure
3. Reference existing workflows or create new ones
4. Run `npm link` to install

### Adding New Workflows

1. Create `get-shit-done/workflows/new-workflow.md`
2. Follow workflow structure with steps
3. Reference from commands

### Adding New Templates

1. Create `get-shit-done/templates/new-template.md`
2. Use placeholder syntax: `[Placeholder]`
3. Reference from workflows

### Adding New References

1. Create `get-shit-done/references/new-reference.md`
2. Include in workflow `<required_reading>` sections

### Customizing Behavior

**Project-level config** (`.planning/config.json`):
```json
{
  "depth": "quick|standard|comprehensive",
  "mode": "interactive|yolo|custom",
  "gates": {
    "execute_next_plan": true,
    "issues_review": true
  }
}
```

---

## Summary

GSD's architecture is built on:

1. **Layered Design**: Commands → Workflows → Templates/References → State
2. **Markdown as Code**: All components are Markdown files
3. **State-Driven**: Project state determines behavior
4. **Extensible**: Easy to add commands, workflows, templates
5. **Git-Integrated**: All changes are committed atomically

This design allows for:
- Easy modification and extension
- Clear separation of concerns
- Consistent file structures
- Predictable state management
- Full project traceability
