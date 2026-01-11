# Installation Guide - Goal Spec Done (GSD)

## Table of Contents

1. [System Requirements](#system-requirements)
2. [Installing Prerequisites](#installing-prerequisites)
3. [Installing GSD](#installing-gsd)
4. [Verifying Installation](#verifying-installation)
5. [Troubleshooting Installation](#troubleshooting-installation)
6. [Updating GSD](#updating-gsd)
7. [Uninstalling GSD](#uninstalling-gsd)

---

## System Requirements

Before installing GSD, ensure your system meets these requirements:

| Requirement | Minimum Version | How to Check |
|-------------|-----------------|--------------|
| Node.js | 18.0.0 | `node --version` |
| NPM | 9.0.0 | `npm --version` |
| Git | 2.30.0 | `git --version` |
| Claude Code | Latest | `claude --version` |

### Operating System Support

| OS | Supported | Notes |
|----|-----------|-------|
| Windows 10/11 | Yes | Use PowerShell or Git Bash |
| macOS 10.15+ | Yes | Use Terminal |
| Ubuntu 20.04+ | Yes | Use Terminal |
| Other Linux | Yes | Most distributions work |

### Hardware Requirements

- **RAM**: 4GB minimum (8GB recommended)
- **Disk Space**: 100MB for GSD + space for your projects
- **Internet**: Required for installation and Claude Code

---

## Installing Prerequisites

### Step 1: Install Node.js and NPM

#### Windows

1. **Download Node.js**
   - Go to https://nodejs.org/
   - Click the "LTS" (Long Term Support) button
   - Download the Windows Installer (.msi)

2. **Run the Installer**
   - Double-click the downloaded file
   - Click "Next" on the welcome screen
   - Accept the license agreement
   - **Important**: Keep all default options checked
   - Click "Install"
   - Click "Finish"

3. **Restart Your Computer**
   - This ensures PATH variables are set correctly

4. **Verify Installation**
   - Open PowerShell (press Win + X, select "Windows PowerShell")
   - Type:
     ```powershell
     node --version
     npm --version
     ```
   - You should see version numbers like `v18.17.0` and `9.6.7`

#### macOS

1. **Option A: Using Homebrew (Recommended)**

   If you have Homebrew installed:
   ```bash
   brew install node
   ```

   If you don't have Homebrew:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   brew install node
   ```

2. **Option B: Using the Installer**
   - Go to https://nodejs.org/
   - Download the macOS Installer (.pkg)
   - Double-click to run
   - Follow the installation wizard

3. **Verify Installation**
   - Open Terminal (press Cmd + Space, type "Terminal")
   - Type:
     ```bash
     node --version
     npm --version
     ```

#### Linux (Ubuntu/Debian)

1. **Update package manager**
   ```bash
   sudo apt update
   ```

2. **Install Node.js**
   ```bash
   # Install Node.js 18.x
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt install -y nodejs
   ```

3. **Verify Installation**
   ```bash
   node --version
   npm --version
   ```

### Step 2: Install Git

#### Windows

1. **Download Git**
   - Go to https://git-scm.com/download/win
   - The download should start automatically

2. **Run the Installer**
   - Double-click the downloaded file
   - Click "Next" through the installer
   - **Important Settings** (keep defaults unless noted):
     - Adjusting PATH: Select "Git from the command line and also from 3rd-party software"
     - Line ending conversions: Select "Checkout Windows-style, commit Unix-style"
   - Click "Install"
   - Click "Finish"

3. **Verify Installation**
   - Open a NEW PowerShell window
   - Type:
     ```powershell
     git --version
     ```

#### macOS

1. **Install Git**
   ```bash
   xcode-select --install
   ```
   A popup will appear. Click "Install".

2. **Verify Installation**
   ```bash
   git --version
   ```

#### Linux (Ubuntu/Debian)

1. **Install Git**
   ```bash
   sudo apt install git
   ```

2. **Verify Installation**
   ```bash
   git --version
   ```

### Step 3: Configure Git

After installing Git, configure your identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Replace with your actual name and email.

### Step 4: Install Claude Code

1. **Visit the Documentation**
   - Go to https://docs.anthropic.com/claude-code
   - Follow the installation instructions for your OS

2. **Create an Anthropic Account** (if you don't have one)
   - Go to https://console.anthropic.com/
   - Sign up for an account
   - Note your API key

3. **Configure Claude Code**
   - Follow the authentication steps in the documentation

4. **Verify Installation**
   ```bash
   claude --version
   ```

---

## Installing GSD

### Method 1: NPM Global Install (Recommended)

This is the simplest method for most users:

```bash
npm install -g goal-spec-done
```

**What happens:**
1. NPM downloads the package
2. The `postinstall` script runs automatically
3. Templates and workflows are copied to `~/.claude/goal-spec-done/`
4. Commands are registered with Claude Code

### Method 2: From Source (For Developers)

If you want to modify GSD or contribute:

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-username/get-shit-done.git
   cd get-shit-done
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Link for Development**
   ```bash
   npm link
   ```

### Method 3: From a Downloaded Archive

If you downloaded a .zip file:

1. **Extract the Archive**
   - Windows: Right-click → "Extract All"
   - macOS: Double-click the .zip
   - Linux: `unzip goal-spec-done.zip`

2. **Navigate to the Directory**
   ```bash
   cd get-shit-done
   ```

3. **Install**
   ```bash
   npm install -g .
   ```

---

## Verifying Installation

### Check 1: Templates Directory

```bash
ls ~/.claude/goal-spec-done/templates/
```

**Expected Output:**
You should see files like:
- `project.md`
- `roadmap.md`
- `state.md`
- `phase-prompt.md`
- (and more)

### Check 2: Workflows Directory

```bash
ls ~/.claude/goal-spec-done/workflows/
```

**Expected Output:**
- `execute-phase.md`
- `plan-phase.md`
- `verify-work.md`

### Check 3: References Directory

```bash
ls ~/.claude/goal-spec-done/references/
```

**Expected Output:**
- `checkpoints.md`
- `principles.md`
- `tdd.md`
- (and more)

### Check 4: Commands Available

```bash
# Start Claude Code
claude

# In Claude Code, type:
/gsd:help
```

**Expected Output:**
A list of all GSD commands with descriptions.

### Full Verification Test

Run this complete test:

```bash
# 1. Create a test directory
mkdir ~/gsd-test
cd ~/gsd-test

# 2. Initialize Git
git init

# 3. Start Claude Code
claude

# 4. In Claude Code, run:
/gsd:new-project

# 5. Answer the prompts:
# - Project name: "Test Project"
# - Description: "Testing GSD installation"
# - Depth: "quick"

# 6. Check that files were created:
ls -la .planning/
```

If you see `PROJECT.md`, `ROADMAP.md`, and `STATE.md`, GSD is working!

---

## Troubleshooting Installation

### Problem: "npm: command not found"

**Cause:** Node.js/NPM is not installed or not in PATH.

**Solution:**
1. Verify Node.js is installed: `node --version`
2. If not installed, follow [Step 1](#step-1-install-nodejs-and-npm)
3. Restart your terminal/computer

### Problem: "Permission denied" during install

**Cause:** You don't have write permission to the install directory.

**Solution (macOS/Linux):**
```bash
# Option 1: Use sudo (not recommended for npm)
sudo npm install -g goal-spec-done

# Option 2: Fix npm permissions (recommended)
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
npm install -g goal-spec-done
```

**Solution (Windows):**
- Run PowerShell as Administrator
- Right-click PowerShell → "Run as Administrator"
- Try the install again

### Problem: "ENOENT" error during install

**Cause:** The postinstall script can't find or create directories.

**Solution:**
1. Manually create the Claude directory:
   ```bash
   mkdir -p ~/.claude/goal-spec-done
   ```
2. Try install again:
   ```bash
   npm install -g goal-spec-done
   ```

### Problem: Commands not found after install

**Cause:** Commands weren't copied to Claude's command directory.

**Solution:**
1. Find where GSD was installed:
   ```bash
   npm list -g goal-spec-done
   ```
2. Manually copy commands:
   ```bash
   # Find the install location
   NPM_ROOT=$(npm root -g)

   # Copy commands
   cp -r $NPM_ROOT/goal-spec-done/commands/* ~/.claude/commands/
   ```

### Problem: "Cannot find module" errors

**Cause:** Dependencies weren't installed correctly.

**Solution:**
```bash
# Uninstall
npm uninstall -g goal-spec-done

# Clear npm cache
npm cache clean --force

# Reinstall
npm install -g goal-spec-done
```

### Problem: Git not initialized error

**Cause:** GSD requires a Git repository.

**Solution:**
```bash
cd your-project-directory
git init
```

### Problem: Claude Code won't start

**Cause:** Claude Code isn't installed or configured.

**Solution:**
1. Verify Claude Code is installed: `claude --version`
2. If not installed, follow [Step 4](#step-4-install-claude-code)
3. If installed but not working, check Anthropic documentation

---

## Updating GSD

### Update to Latest Version

```bash
npm update -g goal-spec-done
```

### Update to Specific Version

```bash
npm install -g goal-spec-done@1.2.3
```

### Check Current Version

```bash
npm list -g goal-spec-done
```

### What Gets Updated

When you update GSD:
- Templates are updated
- Workflows are updated
- References are updated
- Commands are updated
- **Your project data in `.planning/` is NOT affected**

---

## Uninstalling GSD

### Remove the NPM Package

```bash
npm uninstall -g goal-spec-done
```

### Remove GSD Configuration Files

```bash
rm -rf ~/.claude/goal-spec-done
```

### Remove GSD Commands (if needed)

```bash
rm ~/.claude/commands/gsd-*.md
```

### Keep Your Project Data

**Important:** The `.planning/` directory in your projects contains YOUR data. Don't delete it unless you want to lose your project history.

---

## Next Steps

After successful installation:

1. **Read the Quick Start Guide**: `docs/QUICK_START.md`
2. **Try Your First Project**: Create a simple test project
3. **Explore the User Guide**: `docs/USER_GUIDE.md`
4. **Check the Command Reference**: Run `/gsd:help` in Claude Code

Welcome to GSD!
