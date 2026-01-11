# Quick Start Guide - Goal Spec Done (GSD)

## Get Started in 5 Minutes

This guide will have you using GSD within 5 minutes. Let's go!

### Step 1: Install (1 minute)

Make sure you have Node.js installed, then run:

```bash
npm install -g goal-spec-done
```

### Step 2: Create a Folder (30 seconds)

```bash
mkdir my-project
cd my-project
git init
```

### Step 3: Start Claude and Create Project (2 minutes)

```bash
claude
```

Then type:
```
/gsd:new-project
```

Answer the questions Claude asks (project name, description, etc.)

### Step 4: Plan Your First Phase (1 minute)

```
/gsd:plan-phase 1
```

Claude will create a detailed plan for Phase 1.

### Step 5: Execute! (30 seconds to start)

```
/gsd:execute-plan
```

Watch Claude work and verify checkpoints when asked.

**That's it! You're using GSD!**

---

## 10 Educational Use Cases

These examples will teach you GSD's features while building something useful.

---

### Use Case 1: Personal Notes App

**Goal**: Build a simple note-taking application
**What You'll Learn**: Basic project setup and the plan-execute cycle

#### Steps:

**1. Create the project:**
```bash
mkdir notes-app
cd notes-app
git init
claude
```

**2. Initialize GSD:**
```
/gsd:new-project
```

When asked:
- Project name: "Personal Notes"
- Description: "A simple app to save and view notes"
- Depth: "quick"

**3. Check what was created:**
```
/gsd:progress
```

You should see your roadmap with phases!

**4. Plan the first phase:**
```
/gsd:plan-phase 1
```

**5. Execute:**
```
/gsd:execute-plan
```

**What You Learned**:
- How to initialize a GSD project
- The basic plan-execute workflow
- How progress tracking works

---

### Use Case 2: Calculator App

**Goal**: Build a basic calculator
**What You'll Learn**: How checkpoints work for verification

#### Steps:

**1. Create project:**
```bash
mkdir calculator
cd calculator
git init
claude
```

**2. Initialize:**
```
/gsd:new-project
```

- Name: "Simple Calculator"
- Description: "Calculator with add, subtract, multiply, divide"
- Depth: "standard"

**3. Plan first phase:**
```
/gsd:plan-phase 1
```

**4. Execute and watch for checkpoints:**
```
/gsd:execute-plan
```

When you see a checkpoint like:
```
════════════════════════════════════════
CHECKPOINT: Verification Required
════════════════════════════════════════
I built: Calculator with addition function
How to verify: Run `node calculator.js add 2 3`
════════════════════════════════════════
```

Actually test it! Type the command and verify it works.

**What You Learned**:
- Checkpoints pause for your verification
- Always test what Claude builds
- Verification ensures quality

---

### Use Case 3: Weather CLI Tool

**Goal**: Build a command-line weather checker
**What You'll Learn**: How to add phases and modify plans

#### Steps:

**1. Create and initialize:**
```bash
mkdir weather-cli
cd weather-cli
git init
claude
/gsd:new-project
```

- Name: "Weather CLI"
- Description: "Check weather from command line"

**2. View the roadmap:**
```bash
cat .planning/ROADMAP.md
```

**3. Add a new phase:**
```
/gsd:add-phase "Add temperature unit conversion"
```

**4. Check the updated roadmap:**
```
/gsd:progress
```

You should see your new phase added!

**5. Plan and execute Phase 1:**
```
/gsd:plan-phase 1
/gsd:execute-plan
```

**What You Learned**:
- How to add new phases to your project
- How the roadmap tracks all phases
- Phases can be added at any time

---

### Use Case 4: Task Tracker

**Goal**: Build a to-do list with priorities
**What You'll Learn**: Pausing and resuming work

#### Steps:

**1. Create project:**
```bash
mkdir task-tracker
cd task-tracker
git init
claude
/gsd:new-project
```

- Name: "Task Tracker"
- Description: "Track tasks with priorities"

**2. Plan phase 1:**
```
/gsd:plan-phase 1
```

**3. Start execution:**
```
/gsd:execute-plan
```

**4. Midway through, pause the work:**
```
/gsd:pause-work
```

Note the message about where you stopped.

**5. Exit Claude and take a break:**
Type `exit` or close the terminal.

**6. Come back later:**
```bash
cd task-tracker
claude
/gsd:resume-work
```

Claude remembers exactly where you were!

**What You Learned**:
- How to safely pause work
- How resume-work picks up where you left
- Your progress is never lost

---

### Use Case 5: Recipe Book

**Goal**: Build a recipe storage app
**What You'll Learn**: How issues are tracked and handled

#### Steps:

**1. Create and initialize:**
```bash
mkdir recipe-book
cd recipe-book
git init
claude
/gsd:new-project
```

- Name: "Recipe Book"
- Description: "Store and search recipes"

**2. Plan and execute phase 1:**
```
/gsd:plan-phase 1
/gsd:execute-plan
```

**3. While reviewing, think of an improvement:**
```
/gsd:add-todo "Add recipe rating system"
```

**4. Add another idea:**
```
/gsd:add-todo "Add ingredient substitution suggestions"
```

**5. Check your todos:**
```
/gsd:check-todos
```

You'll see both ideas listed!

**6. When ready to implement them:**
```
/gsd:consider-issues
```

**What You Learned**:
- How to capture ideas without interrupting work
- The todo system for future improvements
- How to review and prioritize enhancements

---

### Use Case 6: Quote Generator

**Goal**: Build an inspirational quote generator
**What You'll Learn**: Verify-work for user acceptance testing

#### Steps:

**1. Create project:**
```bash
mkdir quote-generator
cd quote-generator
git init
claude
/gsd:new-project
```

- Name: "Daily Quotes"
- Description: "Random inspirational quote generator"

**2. Plan and execute:**
```
/gsd:plan-phase 1
/gsd:execute-plan
```

**3. After completion, do formal testing:**
```
/gsd:verify-work
```

Claude will guide you through testing:
- Shows a checklist of features to test
- Asks if each one works correctly
- Logs any issues found

**4. For each test, answer honestly:**
- "Pass" if it works
- "Fail" if it doesn't
- "Partial" if it partly works

**5. See the test summary:**

Claude shows:
- How many tests passed
- What issues were found
- What to do next

**What You Learned**:
- How verify-work guides testing
- The importance of user acceptance testing
- How test results are tracked

---

### Use Case 7: Expense Tracker

**Goal**: Build a personal expense tracker
**What You'll Learn**: Decision checkpoints

#### Steps:

**1. Create project:**
```bash
mkdir expense-tracker
cd expense-tracker
git init
claude
/gsd:new-project
```

- Name: "Expense Tracker"
- Description: "Track personal expenses by category"

**2. Plan phase 1:**
```
/gsd:plan-phase 1
```

**3. Execute:**
```
/gsd:execute-plan
```

**4. When you see a decision checkpoint:**
```
════════════════════════════════════════
CHECKPOINT: Decision Needed
════════════════════════════════════════
Decision: How should we store expenses?

Options:
1. json - Simple JSON file (easy, portable)
2. sqlite - SQLite database (structured, queryable)

Select: json or sqlite
════════════════════════════════════════
```

Type your choice: `json` or `sqlite`

Claude continues based on YOUR decision!

**What You Learned**:
- Decision checkpoints let you guide the project
- You control important architectural choices
- Claude explains the trade-offs

---

### Use Case 8: Contact Manager

**Goal**: Build a contact list manager
**What You'll Learn**: Handling plan fixes

#### Steps:

**1. Create and initialize:**
```bash
mkdir contacts
cd contacts
git init
claude
/gsd:new-project
```

- Name: "Contact Manager"
- Description: "Manage personal and work contacts"

**2. Plan and execute phase 1:**
```
/gsd:plan-phase 1
/gsd:execute-plan
```

**3. Run verification:**
```
/gsd:verify-work
```

**4. Mark some tests as "Fail" (pretend you found bugs):**

When Claude asks about each feature:
- Mark one as "Fail"
- Describe the issue

**5. Create a fix plan:**
```
/gsd:plan-fix
```

Claude creates a plan specifically to fix the issues!

**6. Execute the fix:**
```
/gsd:execute-plan
```

**What You Learned**:
- How to report issues during verification
- How plan-fix creates targeted fixes
- The complete feedback loop

---

### Use Case 9: Bookmark Manager

**Goal**: Build a browser bookmark organizer
**What You'll Learn**: Research for complex phases

#### Steps:

**1. Create project:**
```bash
mkdir bookmarks
cd bookmarks
git init
claude
/gsd:new-project
```

- Name: "Bookmark Manager"
- Description: "Organize bookmarks with tags and search"

**2. Before planning a complex phase, do research:**
```
/gsd:research-phase 2
```

Claude will:
- Research best practices
- Find recommended libraries
- Document patterns to use

**3. See the research:**
```bash
cat .planning/phases/02-*/02-RESEARCH.md
```

**4. Now plan with that knowledge:**
```
/gsd:plan-phase 2
```

The plan will be informed by the research!

**What You Learned**:
- Research-phase for complex work
- How research informs planning
- Best practices are captured before coding

---

### Use Case 10: Habit Tracker

**Goal**: Build a daily habit tracking app
**What You'll Learn**: Complete milestone workflow

#### Steps:

**1. Create project:**
```bash
mkdir habits
cd habits
git init
claude
/gsd:new-project
```

- Name: "Habit Tracker"
- Description: "Track daily habits with streaks"
- Depth: "quick" (to have fewer phases)

**2. Plan phase 1:**
```
/gsd:plan-phase 1
```

**3. Execute phase 1:**
```
/gsd:execute-plan
```

**4. Check progress:**
```
/gsd:progress
```

**5. Continue through all phases:**

Repeat for each phase:
```
/gsd:plan-phase [next number]
/gsd:execute-plan
```

**6. When all phases complete:**
```
/gsd:complete-milestone
```

Claude will:
- Archive the completed milestone
- Update the roadmap
- Prepare for next milestone

**7. Start next milestone (if desired):**
```
/gsd:new-milestone
```

**What You Learned**:
- Complete project lifecycle
- How milestones organize major versions
- How to transition between milestones

---

## Command Cheat Sheet

### Getting Started
```
/gsd:new-project       # Start a new project
/gsd:help             # See all commands
```

### Planning
```
/gsd:plan-phase [N]   # Plan phase N
/gsd:add-phase        # Add a new phase
/gsd:research-phase   # Research before planning
```

### Executing
```
/gsd:execute-plan     # Run the current plan
/gsd:verify-work      # Test completed work
```

### Progress
```
/gsd:progress         # See where you are
/gsd:check-todos      # See pending todos
```

### Workflow
```
/gsd:pause-work       # Stop and save state
/gsd:resume-work      # Pick up where you left off
```

### Issues
```
/gsd:add-todo         # Add an idea for later
/gsd:consider-issues  # Review pending issues
/gsd:plan-fix         # Create plan to fix issues
```

---

## Next Steps

Now that you've completed these use cases, you can:

1. **Build your own project** - Use what you learned!
2. **Read the full User Guide** - For deeper understanding
3. **Explore all commands** - Run `/gsd:help` for the full list
4. **Join the community** - Share your projects and get help

Happy building!
