# User Guide - Goal Spec Done (GSD)

## Table of Contents

1. [Welcome to GSD](#welcome-to-gsd)
2. [What You Need Before Starting](#what-you-need-before-starting)
3. [Installing GSD](#installing-gsd)
4. [Your First Project](#your-first-project)
5. [Understanding the Workflow](#understanding-the-workflow)
6. [All Commands Explained](#all-commands-explained)
7. [Working with Phases](#working-with-phases)
8. [Working with Plans](#working-with-plans)
9. [Checkpoints and Verification](#checkpoints-and-verification)
10. [Tracking Progress](#tracking-progress)
11. [Handling Issues](#handling-issues)
12. [Pausing and Resuming Work](#pausing-and-resuming-work)
13. [Tips for Success](#tips-for-success)
14. [Common Questions](#common-questions)
15. [Getting Help](#getting-help)

---

## Welcome to GSD

### What is Goal Spec Done (GSD)?

GSD is a tool that helps you build software projects with AI assistance. Think of it as having a smart project manager and developer assistant combined.

**Here's the basic idea**:
1. You describe what you want to build
2. GSD helps you break it into manageable pieces
3. Claude (the AI) helps execute each piece
4. You verify the work and move forward

### Why Use GSD?

- **Stay organized**: Your project has clear structure from day one
- **Never lose progress**: Everything is tracked and documented
- **Work in chunks**: Complex projects become simple phases
- **Get AI help**: Claude handles the technical details while you guide the vision

### Who is This For?

GSD is for anyone who wants to build software with AI help:
- Solo developers building side projects
- Beginners learning to code
- Experienced developers wanting more structure
- Anyone with a project idea and determination

---

## What You Need Before Starting

### Required Software

Before using GSD, you need to install a few things. Don't worry - we'll guide you through each one!

#### 1. Node.js

**What is it?** A program that runs JavaScript code.

**How to install**:
1. Go to https://nodejs.org/
2. Click the big green "LTS" download button
3. Run the downloaded file
4. Click "Next" through the installer (default options are fine)
5. Restart your computer

**How to verify it worked**:
1. Open your command line:
   - **Windows**: Press `Win + R`, type `cmd`, press Enter
   - **Mac**: Press `Cmd + Space`, type `Terminal`, press Enter
   - **Linux**: Press `Ctrl + Alt + T`
2. Type this and press Enter:
   ```
   node --version
   ```
3. You should see something like `v18.17.0`

#### 2. Git

**What is it?** A tool that tracks changes to your code.

**How to install**:
- **Windows**: Go to https://git-scm.com/download/win, download and run installer
- **Mac**: Open Terminal, type `xcode-select --install`, press Enter
- **Linux**: Open Terminal, type `sudo apt install git`, press Enter

**How to verify it worked**:
```
git --version
```
You should see something like `git version 2.40.0`

#### 3. Claude Code

**What is it?** Anthropic's AI assistant for your command line.

**How to install**:
1. Visit https://docs.anthropic.com/claude-code
2. Follow the installation instructions for your operating system
3. You'll need an Anthropic account (create one if you don't have it)

**How to verify it worked**:
```
claude --version
```

### Basic Skills You'll Need

You should be comfortable with:
- Opening your command line (Terminal on Mac/Linux, Command Prompt on Windows)
- Typing commands and pressing Enter
- Navigating folders (we'll show you how)

---

## Installing GSD

### Step 1: Download GSD

Open your command line and run:

```bash
npm install -g goal-spec-done
```

**What this does**: Downloads GSD and installs it on your computer.

### Step 2: Verify Installation

Run this command:

```bash
ls ~/.claude/goal-spec-done/
```

You should see folders like `templates`, `workflows`, and `references`.

### Step 3: You're Ready!

That's it! GSD is now installed and ready to use.

---

## Your First Project

Let's create your first project together. We'll make a simple to-do list application.

### Step 1: Create a Project Folder

```bash
# Create a new folder
mkdir my-first-project

# Go into that folder
cd my-first-project

# Initialize Git (required for GSD)
git init
```

### Step 2: Start Claude Code

```bash
claude
```

You should see Claude Code start up and show a prompt.

### Step 3: Initialize Your Project

Type this command:

```
/gsd:new-project
```

Claude will ask you some questions:
- **What's your project name?** → "My Todo App"
- **Brief description?** → "A simple app to track my tasks"
- **What depth?** → Choose "standard" for now

### Step 4: See What Was Created

After Claude finishes, look at your project:

```bash
ls -la .planning/
```

You should see files like:
- `PROJECT.md` - Describes your project
- `ROADMAP.md` - Shows the phases
- `STATE.md` - Tracks where you are

### Congratulations!

You just created your first GSD project! Now let's learn how to work with it.

---

## Understanding the Workflow

### The GSD Cycle

Every project follows this cycle:

```
  ┌─────────────────────────────────────────────────────────┐
  │                                                         │
  │   1. PLAN          2. EXECUTE          3. VERIFY        │
  │   /gsd:plan-phase  /gsd:execute-plan   /gsd:verify-work │
  │                                                         │
  │       ▼                 ▼                   ▼           │
  │   Create plan      Do the work         Check it works   │
  │   for phase        with Claude's       with your eyes   │
  │                    help                                 │
  │                                                         │
  └─────────────────────────────────────────────────────────┘
                              │
                              ▼
                    Move to next phase
                    (repeat the cycle)
```

### What is a Phase?

A **phase** is a chunk of work with a clear goal. For example:
- Phase 1: Set up the project foundation
- Phase 2: Create user authentication
- Phase 3: Build the main features
- Phase 4: Add polish and testing

### What is a Plan?

A **plan** is a detailed set of tasks within a phase. Each phase can have multiple plans. For example:

Phase 2 (Authentication) might have:
- Plan 01: Create login form
- Plan 02: Handle registration
- Plan 03: Password reset

### What is a Task?

A **task** is a single piece of work. Each plan has 2-3 tasks. For example:

Plan 01 (Login form) might have:
- Task 1: Create the login page HTML
- Task 2: Add form validation
- Task 3: Connect to authentication API

---

## All Commands Explained

### Project Setup Commands

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/gsd:new-project` | Creates a new GSD project | Starting a brand new project |
| `/gsd:create-roadmap` | Creates the phase roadmap | After discussing your project |
| `/gsd:map-codebase` | Analyzes existing code | When adding GSD to existing project |

### Phase Management Commands

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/gsd:plan-phase [N]` | Creates detailed plan for phase N | Before starting a phase |
| `/gsd:execute-plan` | Runs the plan | After planning is done |
| `/gsd:add-phase` | Adds a new phase | When you need more phases |
| `/gsd:insert-phase` | Inserts urgent phase | For emergency work |
| `/gsd:remove-phase` | Removes a phase | When phase is no longer needed |
| `/gsd:discuss-phase [N]` | Talk about phase goals | When unsure what phase should do |
| `/gsd:research-phase [N]` | Deep research for complex phase | For technical phases |

### Progress Commands

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/gsd:progress` | Shows where you are | Anytime you're curious |
| `/gsd:verify-work` | Test what was built | After completing work |

### Workflow Commands

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/gsd:pause-work` | Saves state for later | When you need to stop |
| `/gsd:resume-work` | Picks up where you left off | When returning to work |
| `/gsd:resume-task` | Resume interrupted task | After a crash or timeout |

### Issue Management Commands

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/gsd:add-todo` | Add a future task | When you think of something |
| `/gsd:check-todos` | Review all todos | When planning next work |
| `/gsd:consider-issues` | Review logged issues | When deciding what to fix |
| `/gsd:plan-fix` | Create plan to fix issues | After finding bugs |

### Milestone Commands

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/gsd:new-milestone` | Start a new milestone | After completing major version |
| `/gsd:discuss-milestone` | Define milestone goals | Before planning milestone |
| `/gsd:complete-milestone` | Archive finished milestone | When all phases done |

### Help Commands

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/gsd:help` | Shows all commands | When you need guidance |
| `/gsd:list-phase-assumptions` | Shows assumptions made | To review what Claude assumed |

---

## Working with Phases

### Starting a New Phase

When you're ready to start Phase 1:

```
/gsd:plan-phase 1
```

Claude will:
1. Read your project goals
2. Break the phase into tasks
3. Create a detailed plan
4. Show you what will be done

### Reviewing the Plan

Before executing, you can review:

```bash
cat .planning/phases/01-foundation/01-01-PLAN.md
```

### Making Changes to the Plan

If you want changes:
1. Tell Claude what to change
2. Claude will update the plan
3. Review again until satisfied

### Adding More Phases

If you need another phase:

```
/gsd:add-phase "Add dark mode theme"
```

### Inserting Urgent Phases

If something urgent comes up:

```
/gsd:insert-phase 2 "Fix critical security bug"
```

This creates Phase 2.1 between Phase 2 and Phase 3.

---

## Working with Plans

### Executing a Plan

Once you're happy with a plan:

```
/gsd:execute-plan
```

Claude will:
1. Read the plan
2. Execute each task
3. Commit the code
4. Create a summary

### What Happens During Execution

For each task, Claude:
1. **Does the work**: Creates/modifies files
2. **Verifies it worked**: Runs tests or checks
3. **Commits the change**: Saves to Git

### When Claude Finds Problems

Claude follows these rules:

1. **Bugs** → Fixed immediately
2. **Missing essential features** → Added immediately
3. **Blocking issues** → Fixed to continue
4. **Big architectural changes** → Asks you first
5. **Nice-to-haves** → Logged for later

### After a Plan Completes

You'll see a summary like:

```
Plan 01-01 complete!
Summary: .planning/phases/01-foundation/01-01-SUMMARY.md

Tasks completed: 3/3
- Created user model
- Added authentication
- Set up database

Next: /gsd:execute-plan for 01-02
```

---

## Checkpoints and Verification

### What are Checkpoints?

Checkpoints are moments where Claude pauses for your input.

### Types of Checkpoints

#### 1. Verification Checkpoints (Most Common)

Claude says: "I built X. Please verify it works."

**What to do**:
1. Look at what Claude built
2. Test it manually
3. Type "approved" or describe issues

**Example**:
```
════════════════════════════════════════
CHECKPOINT: Verification Required
════════════════════════════════════════

I built: Login page with email/password form

How to verify:
1. Open http://localhost:3000/login
2. Check that form displays correctly
3. Try entering test credentials

Type "approved" or describe issues
════════════════════════════════════════
```

#### 2. Decision Checkpoints

Claude asks you to make a choice.

**What to do**:
1. Read the options
2. Type your choice

**Example**:
```
════════════════════════════════════════
CHECKPOINT: Decision Needed
════════════════════════════════════════

Decision: How should we store user sessions?

Options:
1. jwt - JSON Web Tokens (stateless, scalable)
2. sessions - Database sessions (traditional, simple)

Select: jwt or sessions
════════════════════════════════════════
```

#### 3. Action Checkpoints (Rare)

Claude needs you to do something it can't.

**What to do**:
1. Follow the instructions
2. Type "done" when complete

**Example**:
```
════════════════════════════════════════
CHECKPOINT: Your Action Needed
════════════════════════════════════════

I need: Two-factor authentication code

Instructions:
1. Check your email for the code
2. Enter it on the website

Type "done" when complete
════════════════════════════════════════
```

---

## Tracking Progress

### Checking Your Progress

At any time, run:

```
/gsd:progress
```

You'll see something like:

```
Project: My Todo App
═══════════════════════════════

Current Position:
Phase: 2 of 4 (User Authentication)
Plan: 1 of 2 in current phase
Status: In progress

Progress: ████████░░░░░░░░░░░░ 40%

Last Activity: 2024-01-15 - Completed 01-03-PLAN.md

Next Up:
- Complete 02-01-PLAN.md (Login system)
- Then: 02-02-PLAN.md (Registration)
```

### Understanding the Progress Bar

```
░░░░░░░░░░░░░░░░░░░░ 0%    - Just started
████░░░░░░░░░░░░░░░░ 20%   - Early progress
████████░░░░░░░░░░░░ 40%   - Getting there
████████████░░░░░░░░ 60%   - Past halfway!
████████████████░░░░ 80%   - Almost done
████████████████████ 100%  - Complete!
```

### Understanding STATE.md

The `STATE.md` file tracks:
- Where you are in the project
- Recent decisions made
- Issues to address
- How to resume work

You can read it anytime:

```bash
cat .planning/STATE.md
```

---

## Handling Issues

### What are Issues?

Issues are problems or improvements discovered during work:
- Bugs that aren't critical
- Ideas for improvements
- Things to do later

### How Issues Get Created

During execution, if Claude finds something:
- Critical bugs → Fixed immediately
- Non-critical items → Logged as issues

### Viewing Issues

```
/gsd:consider-issues
```

Shows all pending issues with details.

### Creating Fix Plans

When you want to address issues:

```
/gsd:plan-fix
```

This creates a plan to fix logged issues.

### Adding Your Own Ideas

When you think of something:

```
/gsd:add-todo "Add export to PDF feature"
```

### Reviewing All Todos

```
/gsd:check-todos
```

Shows all todos you've added.

---

## Pausing and Resuming Work

### When You Need to Stop

Life happens! When you need to pause:

```
/gsd:pause-work
```

This saves your exact position so you can resume later.

### When You Come Back

Start Claude Code and run:

```
/gsd:resume-work
```

Claude will:
1. Read where you left off
2. Show you the context
3. Continue from that point

### If Claude Timed Out

If Claude stopped mid-task:

```
/gsd:resume-task
```

This resumes the specific task that was interrupted.

---

## Tips for Success

### Tip 1: Start Small

Don't try to build everything at once. Start with:
- A simple first phase
- Clear, achievable goals
- Quick wins to build momentum

### Tip 2: Review Before Executing

Always review plans before running `/gsd:execute-plan`:
- Read the PLAN.md file
- Understand what will happen
- Suggest changes if needed

### Tip 3: Verify Thoroughly

During verification checkpoints:
- Actually test the feature
- Don't just approve blindly
- Report any issues you find

### Tip 4: Keep Plans Small

Big plans = potential problems. Each plan should:
- Have 2-3 tasks
- Complete in one session
- Have a clear purpose

### Tip 5: Use Descriptive Names

When creating projects and phases:
- Be specific: "User Authentication" not "Phase 2"
- Be clear: "Add search feature" not "Improvements"

### Tip 6: Commit Often

GSD commits after each task, which means:
- You can undo specific changes
- Your history is clear
- Nothing gets lost

### Tip 7: Take Breaks

It's better to pause properly than to rush:
- Use `/gsd:pause-work` when tired
- Resume fresh the next day
- Quality over speed

---

## Common Questions

### Q: How long does each phase take?

**A:** It depends on complexity, but expect:
- Simple phases: 15-30 minutes
- Medium phases: 30-60 minutes
- Complex phases: 1-2 hours

### Q: Can I modify files manually?

**A:** Yes, but:
- Tell Claude what you changed
- Be aware it might conflict with plans
- Best to let Claude handle most changes

### Q: What if Claude makes a mistake?

**A:** Several options:
1. Tell Claude to fix it
2. Use Git to undo changes
3. Log it as an issue for later

### Q: Can I use GSD with existing projects?

**A:** Yes! Run:
```
/gsd:map-codebase
```
Then create a roadmap for new work.

### Q: How do I change the plan after it's created?

**A:** Just tell Claude:
- "Please modify task 2 to..."
- "Can we add a task for..."
- "Remove the third task"

### Q: What if I don't understand a plan?

**A:** Ask Claude:
- "Explain task 1 in simple terms"
- "What files will this change?"
- "Why are we doing it this way?"

### Q: Can multiple people work on the same project?

**A:** GSD is designed for solo work, but you can:
- Each person works on different phases
- Coordinate through Git branches
- Share the `.planning/` directory

### Q: How do I know if GSD is working correctly?

**A:** Check for:
- Files in `.planning/` directory
- Progress shown by `/gsd:progress`
- Git commits being created

---

## Getting Help

### Built-in Help

```
/gsd:help
```

Shows all available commands with descriptions.

### Reading Documentation

```bash
# View your project state
cat .planning/STATE.md

# View the roadmap
cat .planning/ROADMAP.md

# View current plan
cat .planning/phases/[phase-name]/*-PLAN.md
```

### Asking Claude

You can always ask Claude:
- "What should I do next?"
- "Explain what just happened"
- "How do I fix this issue?"

### Reporting Problems

If you find a bug in GSD:
1. Note what command you ran
2. Note what error you saw
3. Check the project's issue tracker

---

## Congratulations!

You now know how to use GSD! Here's a quick recap:

1. **Install**: `npm install -g goal-spec-done`
2. **Create project**: `/gsd:new-project`
3. **Plan phases**: `/gsd:plan-phase [N]`
4. **Execute plans**: `/gsd:execute-plan`
5. **Verify work**: `/gsd:verify-work`
6. **Track progress**: `/gsd:progress`
7. **Take breaks**: `/gsd:pause-work` and `/gsd:resume-work`

Now go build something amazing!
