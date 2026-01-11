# Developer Guide - Goal Spec Done (GSD)

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Understanding the Architecture](#understanding-the-architecture)
4. [Setting Up Your Development Environment](#setting-up-your-development-environment)
5. [Project Structure Deep Dive](#project-structure-deep-dive)
6. [Core Concepts Explained](#core-concepts-explained)
7. [How Commands Work](#how-commands-work)
8. [How Workflows Work](#how-workflows-work)
9. [How Templates Work](#how-templates-work)
10. [Making Your First Change](#making-your-first-change)
11. [Testing Your Changes](#testing-your-changes)
12. [Common Development Tasks](#common-development-tasks)
13. [Debugging Guide](#debugging-guide)
14. [Best Practices](#best-practices)
15. [Troubleshooting](#troubleshooting)

---

## Introduction

### What is GSD?

Goal Spec Done (GSD) is a **workflow framework** for Claude Code. Think of it as a project management system that works inside your terminal, powered by AI.

**Key Idea**: Instead of manually organizing your development work, you use slash commands (like `/gsd:plan-phase`) and Claude helps you plan, execute, and track your progress.

### Who is This Guide For?

This guide is for developers who:
- Want to understand how GSD works internally
- Want to modify or extend GSD's functionality
- Have programming experience but may be new to:
  - Node.js and NPM
  - Markdown-based tooling
  - Claude Code and AI-assisted development

### What You'll Learn

By the end of this guide, you'll understand:
- How GSD's commands, workflows, and templates work together
- How to modify existing commands
- How to add new commands
- How to test your changes
- How to debug issues

---

## Prerequisites

### Required Software

Before you begin, ensure you have these installed:

#### 1. Node.js (Version 18 or higher)

**What is Node.js?** Node.js is a JavaScript runtime that lets you run JavaScript outside a web browser.

**How to check if you have it:**
```bash
node --version
```

If you see something like `v18.17.0` or higher, you're good!

**How to install it:**
- **Windows**: Download from https://nodejs.org/ and run the installer
- **Mac**: `brew install node` (if you have Homebrew) or download from https://nodejs.org/
- **Linux**: `sudo apt install nodejs npm` (Ubuntu/Debian) or use your package manager

#### 2. NPM (Comes with Node.js)

**What is NPM?** NPM (Node Package Manager) is a tool for installing and managing JavaScript packages.

**How to check:**
```bash
npm --version
```

You should see a version number like `9.6.7`.

#### 3. Git

**What is Git?** Git is a version control system that tracks changes to your code.

**How to check:**
```bash
git --version
```

**How to install:**
- **Windows**: Download from https://git-scm.com/download/win
- **Mac**: `xcode-select --install` or `brew install git`
- **Linux**: `sudo apt install git`

#### 4. Claude Code CLI

**What is Claude Code?** Claude Code is Anthropic's command-line interface for Claude AI.

**How to install:**
Follow the official installation guide at: https://docs.anthropic.com/claude-code

#### 5. A Text Editor

You need a text editor to modify files. Recommended options:
- **VS Code** (free, popular): https://code.visualstudio.com/
- **Sublime Text**: https://www.sublimetext.com/
- **Notepad++** (Windows): https://notepad-plus-plus.org/

### Required Knowledge

You should be familiar with:
- Basic command line usage (navigating directories, running commands)
- Basic programming concepts (variables, functions, files)
- Markdown syntax (headers, lists, code blocks)

---

## Understanding the Architecture

### The Big Picture

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER                                     │
│                    (You, the developer)                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ Types command like /gsd:plan-phase
┌─────────────────────────────────────────────────────────────────┐
│                      CLAUDE CODE                                 │
│                   (AI-powered CLI)                               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ Reads command definition
┌─────────────────────────────────────────────────────────────────┐
│                    COMMANDS (24 files)                           │
│              commands/gsd/*.md                                   │
│                                                                  │
│  Examples: plan-phase.md, execute-plan.md, verify-work.md        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ Commands reference workflows
┌─────────────────────────────────────────────────────────────────┐
│                   WORKFLOWS (4 files)                            │
│           goal-spec-done/workflows/*.md                           │
│                                                                  │
│  Detailed step-by-step instructions for Claude                   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ Workflows use templates
┌─────────────────────────────────────────────────────────────────┐
│                   TEMPLATES (14 files)                           │
│           goal-spec-done/templates/*.md                           │
│                                                                  │
│  Structure definitions for project files                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ Creates/modifies files
┌─────────────────────────────────────────────────────────────────┐
│                  YOUR PROJECT                                    │
│              .planning/ directory                                │
│                                                                  │
│  PROJECT.md, ROADMAP.md, STATE.md, phases/                       │
└─────────────────────────────────────────────────────────────────┘
```

### Data Flow Example

Let's trace what happens when you run `/gsd:plan-phase 3`:

1. **You type**: `/gsd:plan-phase 3`
2. **Claude Code reads**: `commands/gsd/plan-phase.md`
3. **Command says**: "Use workflow at `workflows/plan-phase.md`"
4. **Workflow instructs Claude to**:
   - Read `STATE.md` and `ROADMAP.md`
   - Understand Phase 3's goals
   - Break work into tasks
   - Create a PLAN.md file using the template
5. **Template provides**: Structure for PLAN.md
6. **Result**: `.planning/phases/03-name/03-01-PLAN.md` is created

---

## Setting Up Your Development Environment

### Step 1: Clone the Repository

**What is "cloning"?** Cloning creates a copy of the project on your computer.

```bash
# Navigate to where you want the project
cd ~/projects  # or wherever you keep projects

# Clone the repository
git clone https://github.com/your-username/goal-spec-done_me.git

# Enter the project directory
cd goal-spec-done_me
```

### Step 2: Understand the Installation

GSD uses NPM's `postinstall` script to copy files to your Claude Code configuration.

**Important**: The `bin/install.js` file runs automatically when you do `npm install`.

Look at `package.json`:
```json
{
  "scripts": {
    "postinstall": "node bin/install.js"
  }
}
```

### Step 3: Install the Package Locally

For development, use `npm link` to create a symbolic link:

```bash
# In the goal-spec-done_me directory
npm link
```

**What does this do?** It creates a global symlink so changes you make are immediately available.

### Step 4: Verify Installation

Check that files were copied correctly:

```bash
# Check if templates exist
ls ~/.claude/goal-spec-done/templates/

# Check if workflows exist
ls ~/.claude/goal-spec-done/workflows/

# Check if references exist
ls ~/.claude/goal-spec-done/references/
```

You should see files in each directory.

### Step 5: Create a Test Project

Create a separate directory for testing:

```bash
mkdir ~/test-gsd-project
cd ~/test-gsd-project
git init
```

Now you can test GSD commands here without affecting real projects.

---

## Project Structure Deep Dive

Let's explore every directory and file:

### Root Directory

```
goal-spec-done_me/
├── bin/                    # Installation scripts
├── commands/               # Slash command definitions
├── goal-spec-done/          # Core framework files
├── .planning/              # Example project state (for testing)
├── docs/                   # Documentation (you're reading this!)
├── package.json            # NPM configuration
├── LICENSE                 # MIT License
├── README.md               # Project overview
└── CLAUDE.md               # Instructions for Claude
```

### The `bin/` Directory

```
bin/
└── install.js              # Post-install script
```

**install.js** - This script runs after `npm install`:

```javascript
// What it does:
// 1. Creates ~/.claude/goal-spec-done/ if it doesn't exist
// 2. Copies templates/, workflows/, references/ there
// 3. Copies commands/gsd/ to Claude's commands directory
```

### The `commands/gsd/` Directory

```
commands/gsd/
├── add-phase.md           # Add a new phase to roadmap
├── add-todo.md            # Add a todo for later
├── check-todos.md         # Review pending todos
├── complete-milestone.md  # Archive completed milestone
├── consider-issues.md     # Review deferred issues
├── create-roadmap.md      # Create project roadmap
├── discuss-milestone.md   # Define milestone vision
├── discuss-phase.md       # Define phase vision
├── execute-plan.md        # Execute a plan
├── help.md                # Show help information
├── insert-phase.md        # Insert urgent phase
├── list-phase-assumptions.md  # List assumptions made
├── map-codebase.md        # Map existing codebase
├── new-milestone.md       # Start new milestone
├── new-project.md         # Initialize new project
├── pause-work.md          # Pause and save state
├── plan-fix.md            # Plan fixes for issues
├── plan-phase.md          # Create phase plan
├── progress.md            # Show current progress
├── remove-phase.md        # Remove a phase
├── research-phase.md      # Deep research for phase
├── resume-task.md         # Resume interrupted task
├── resume-work.md         # Resume after break
└── verify-work.md         # User acceptance testing
```

### The `goal-spec-done/` Directory

```
goal-spec-done/
├── references/            # Reference documentation
├── templates/             # File templates
└── workflows/             # Detailed workflows
```

#### References (`goal-spec-done/references/`)

```
references/
├── checkpoints.md         # How checkpoints work
├── continuation-format.md # How to continue work
├── git-integration.md     # Git commit conventions
├── plan-format.md         # PLAN.md structure
├── principles.md          # Core principles
├── questioning.md         # How to ask questions
├── research-pitfalls.md   # Research best practices
├── scope-estimation.md    # Task sizing guidance
└── tdd.md                 # Test-driven development
```

#### Templates (`goal-spec-done/templates/`)

```
templates/
├── agent-history.md       # Subagent tracking
├── config.json            # Project configuration
├── context.md             # Phase context template
├── continue-here.md       # Continuation point
├── discovery.md           # Discovery documentation
├── issues.md              # Issues tracking
├── milestone-archive.md   # Archived milestone
├── milestone-context.md   # Milestone vision
├── milestone.md           # Milestone template
├── phase-prompt.md        # PLAN.md template
├── project.md             # PROJECT.md template
├── research.md            # Research documentation
├── roadmap.md             # ROADMAP.md template
├── state.md               # STATE.md template
├── summary.md             # SUMMARY.md template
└── uat-issues.md          # UAT issues template
```

#### Workflows (`goal-spec-done/workflows/`)

```
workflows/
├── execute-phase.md       # How to execute plans
├── plan-phase.md          # How to create plans
└── verify-work.md         # How to verify work
```

---

## Core Concepts Explained

### Concept 1: Plans Are Prompts

**Traditional approach**: Documentation tells humans what to do
**GSD approach**: PLAN.md files ARE instructions for Claude to execute

Example PLAN.md structure:
```markdown
---
phase: 01-foundation
plan: 01
type: standard
---

<objective>
Create user authentication system
</objective>

<tasks>
<task name="Create User Model" type="auto">
**Files:** src/models/User.ts
**Action:** Create User model with email and password fields
**Verify:** TypeScript compiles without errors
**Done:** User model exists with correct fields
</task>
</tasks>
```

### Concept 2: The Planning Directory

Every GSD project has a `.planning/` directory:

```
.planning/
├── PROJECT.md     # What this project is
├── ROADMAP.md     # All phases and plans
├── STATE.md       # Where we are now
├── ISSUES.md      # Deferred issues
├── config.json    # Project settings
└── phases/        # Phase-specific files
    ├── 01-foundation/
    │   ├── 01-01-PLAN.md
    │   └── 01-01-SUMMARY.md
    └── 02-features/
        └── 02-01-PLAN.md
```

### Concept 3: Context Management

**Problem**: Claude has limited "memory" (context window)
**Solution**: Keep plans small (2-3 tasks, ~50% context usage)

Why this matters:
- Large plans = Claude forgets earlier instructions
- Small plans = Consistent quality throughout

### Concept 4: Deviation Rules

When executing plans, Claude may find unexpected work. Five rules handle this:

| Rule | Trigger | Action |
|------|---------|--------|
| 1 | Bug found | Fix immediately |
| 2 | Missing critical feature | Add immediately |
| 3 | Blocking issue | Fix to unblock |
| 4 | Architectural change | Ask user first |
| 5 | Nice-to-have | Log for later |

### Concept 5: Checkpoints

Checkpoints pause execution for human input:

| Type | Purpose | Example |
|------|---------|---------|
| `checkpoint:human-verify` | User checks work | "Verify the login page looks correct" |
| `checkpoint:decision` | User makes choice | "Choose: JWT or sessions?" |
| `checkpoint:human-action` | User does something | "Click the 2FA link in your email" |

---

## How Commands Work

### Command File Structure

Every command in `commands/gsd/` follows this pattern:

```markdown
# Command description (shown in help)

<arguments>
$1 - First argument description
$2 - Second argument description (optional)
</arguments>

<process>
Step-by-step instructions for Claude
</process>

<success_criteria>
- [ ] What must be true when done
</success_criteria>
```

### Example: Analyzing `progress.md`

```markdown
Show current project progress and status.

<process>
1. Read .planning/STATE.md
2. Read .planning/ROADMAP.md
3. Calculate completion percentage
4. Display formatted progress
</process>

<success_criteria>
- [ ] Current phase/plan displayed
- [ ] Progress bar shown
- [ ] Next steps suggested
</success_criteria>
```

### Creating a New Command

**Step 1**: Create new file `commands/gsd/my-command.md`

**Step 2**: Add command structure:

```markdown
# My Command - Brief description

<arguments>
$1 - What the first argument is
</arguments>

<process>
1. First thing Claude should do
2. Second thing Claude should do
3. What to output to user
</process>

<success_criteria>
- [ ] Criterion 1
- [ ] Criterion 2
</success_criteria>
```

**Step 3**: Reinstall to copy new command:

```bash
npm link
```

**Step 4**: Test in Claude Code:

```bash
cd ~/test-gsd-project
claude  # Start Claude Code
# Type: /gsd:my-command
```

---

## How Workflows Work

### Workflow File Structure

Workflows use XML-like tags for organization:

```markdown
<purpose>
What this workflow accomplishes
</purpose>

<required_reading>
Files Claude must read first
</required_reading>

<process>

<step name="step_name">
What to do in this step
</step>

<step name="another_step">
What to do next
</step>

</process>

<success_criteria>
- [ ] What must be true when workflow completes
</success_criteria>
```

### Example: Simplified `execute-phase.md`

```markdown
<purpose>
Execute a plan and create summary
</purpose>

<process>

<step name="load_project_state">
Read .planning/STATE.md to understand context
</step>

<step name="identify_plan">
Find which plan to execute next
</step>

<step name="execute">
For each task in the plan:
1. Do the work
2. Verify it worked
3. Commit the change
</step>

<step name="create_summary">
Create SUMMARY.md documenting what was done
</step>

</process>
```

### Modifying Workflows

**Important**: Workflow changes affect ALL commands that use them.

Before modifying:
1. Understand which commands use this workflow
2. Test thoroughly after changes
3. Consider backwards compatibility

---

## How Templates Work

### Template File Structure

Templates use placeholder syntax:

```markdown
# [Project Name]

## What This Is

[Description of the project]

## Requirements

### Active

- [ ] [Requirement 1]
- [ ] [Requirement 2]

## Constraints

- **[Type]**: [Constraint description]
```

### Example: Simplified `state.md` Template

```markdown
# Project State

## Current Position

Phase: [X] of [Y] ([Phase name])
Plan: [A] of [B] in current phase
Status: [Ready/In progress/Complete]
Last activity: [Date] - [What happened]

Progress: [░░░░░░░░░░] 0%

## Session Continuity

Last session: [Date and time]
Stopped at: [What was last completed]
```

### Using Templates in Workflows

Templates are referenced like this:

```markdown
<step name="create_file">
Use template from ~/.claude/goal-spec-done/templates/state.md
to create .planning/STATE.md
</step>
```

---

## Making Your First Change

Let's add a simple enhancement to the `progress` command.

### Goal

Make `/gsd:progress` also show how many issues are pending.

### Step 1: Find the Command File

```bash
cat commands/gsd/progress.md
```

### Step 2: Understand Current Behavior

Read through the file to understand what it currently does.

### Step 3: Plan Your Change

We want to add:
- Check if ISSUES.md exists
- Count open issues
- Display the count

### Step 4: Make the Change

Edit `commands/gsd/progress.md`:

```markdown
# (Add to the <process> section)

4. Check for pending issues:
   ```bash
   cat .planning/ISSUES.md 2>/dev/null | grep -c "^### ISS-"
   ```
   Display: "Open issues: [count]"
```

### Step 5: Test Your Change

```bash
# Reinstall
npm link

# Test
cd ~/test-gsd-project
claude
# Type: /gsd:progress
```

### Step 6: Verify It Works

The progress output should now include issue count.

---

## Testing Your Changes

### Manual Testing Workflow

1. **Create test project**:
   ```bash
   mkdir ~/test-gsd
   cd ~/test-gsd
   git init
   ```

2. **Reinstall package**:
   ```bash
   cd ~/path/to/goal-spec-done_me
   npm link
   ```

3. **Test in Claude Code**:
   ```bash
   cd ~/test-gsd
   claude
   # Run your command
   ```

4. **Check results**:
   - Did files get created correctly?
   - Is the output formatted right?
   - Do error cases work?

### Testing Checklist

For any change, verify:

- [ ] Command runs without errors
- [ ] Expected files are created
- [ ] File contents are correct
- [ ] Error messages are helpful
- [ ] Edge cases are handled

### Common Test Scenarios

| Scenario | How to Test |
|----------|-------------|
| Fresh project | Run `/gsd:new-project` in empty directory |
| Mid-project | Create `.planning/` manually, test commands |
| Error handling | Provide invalid arguments |
| Missing files | Delete expected files, run command |

---

## Common Development Tasks

### Task 1: Adding a New Template

**Step 1**: Create template file

```bash
touch goal-spec-done/templates/my-template.md
```

**Step 2**: Add content with placeholders

```markdown
# [Title]

## Section 1

[Content for section 1]

## Section 2

- **[Field]**: [Value]
```

**Step 3**: Reference in a workflow

```markdown
<step name="create_file">
Use template from ~/.claude/goal-spec-done/templates/my-template.md
</step>
```

**Step 4**: Reinstall and test

```bash
npm link
```

### Task 2: Modifying an Existing Command

**Step 1**: Find the command

```bash
ls commands/gsd/ | grep keyword
```

**Step 2**: Read and understand it

```bash
cat commands/gsd/command-name.md
```

**Step 3**: Make changes

**Step 4**: Test thoroughly

### Task 3: Adding a New Reference Document

**Step 1**: Create the file

```bash
touch goal-spec-done/references/my-reference.md
```

**Step 2**: Add helpful content

**Step 3**: Reference from workflows/commands where needed

### Task 4: Debugging a Workflow

**Step 1**: Find where it's failing

- Check Claude Code output for errors
- Look at what files were/weren't created

**Step 2**: Read the workflow step-by-step

```bash
cat goal-spec-done/workflows/workflow-name.md
```

**Step 3**: Add debug output (temporarily)

```markdown
<step name="debug">
Print current state to help debug:
```bash
echo "Current directory: $(pwd)"
echo "Files exist:"
ls -la .planning/
```
</step>
```

---

## Debugging Guide

### Problem: Command Not Found

**Symptom**: Claude says it doesn't recognize `/gsd:command`

**Causes and Solutions**:

1. **Command file missing**
   ```bash
   ls commands/gsd/command.md
   # If not found, create it
   ```

2. **Package not installed**
   ```bash
   npm link  # Reinstall
   ```

3. **Claude needs restart**
   ```bash
   # Exit Claude Code and restart
   exit
   claude
   ```

### Problem: Files Not Created

**Symptom**: Expected files in `.planning/` don't exist

**Causes and Solutions**:

1. **Directory doesn't exist**
   ```bash
   mkdir -p .planning/phases
   ```

2. **Workflow step missing**
   - Check the workflow for file creation step
   - Verify template path is correct

3. **Git not initialized**
   ```bash
   git init
   ```

### Problem: Template Content Wrong

**Symptom**: Created files have incorrect content

**Causes and Solutions**:

1. **Template has errors**
   - Review template file
   - Check placeholder syntax

2. **Workflow not substituting values**
   - Check that workflow passes correct values

### Problem: Git Commit Fails

**Symptom**: "nothing to commit" or commit errors

**Causes and Solutions**:

1. **No files staged**
   ```bash
   git add .planning/file.md
   ```

2. **Git not configured**
   ```bash
   git config user.email "you@example.com"
   git config user.name "Your Name"
   ```

---

## Best Practices

### Code Organization

1. **Keep commands focused**: One command = one purpose
2. **Reuse workflows**: Don't duplicate logic across commands
3. **Document everything**: Future you will thank present you

### Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Commands | kebab-case | `plan-phase.md` |
| Templates | kebab-case | `project.md` |
| Placeholders | [Brackets] | `[Project Name]` |
| Step names | snake_case | `<step name="load_state">` |

### Testing Practices

1. **Test edge cases**: Empty projects, missing files, invalid input
2. **Test full workflows**: Not just individual commands
3. **Keep test projects**: Reuse them for regression testing

### Version Control

1. **Commit often**: Small, focused commits
2. **Write clear messages**: Describe WHAT and WHY
3. **Create branches**: For experimental changes

---

## Troubleshooting

### "Cannot find module" Error

```bash
# Reinstall dependencies
npm install

# Reinstall the package
npm link
```

### "Permission denied" Error

```bash
# On Mac/Linux
chmod +x bin/install.js

# Check directory permissions
ls -la ~/.claude/
```

### "ENOENT" Error (File Not Found)

```bash
# Check if the file exists
ls -la path/to/file

# Check if directory exists
ls -la path/to/directory/
```

### Commands Work Sometimes But Not Always

1. **Clear Claude Code cache**: Exit and restart
2. **Check for syntax errors**: Validate Markdown
3. **Check file encodings**: Should be UTF-8

### Still Stuck?

1. Re-read this guide's relevant section
2. Check the example `.planning/` directory
3. Review similar existing commands for patterns
4. Create a minimal reproduction case

---

## Next Steps

Now that you understand GSD's internals:

1. **Explore the code**: Read through existing commands and workflows
2. **Make small changes**: Start with simple modifications
3. **Test thoroughly**: Always verify your changes work
4. **Document your work**: Update docs when you add features
5. **Share improvements**: Consider contributing back to the project

Happy coding!
