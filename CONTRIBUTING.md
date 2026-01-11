# Contributing to Get Shit Done (GSD)

Thank you for your interest in contributing to GSD! This document provides guidelines and instructions for contributors.

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [How to Contribute](#how-to-contribute)
3. [Development Setup](#development-setup)
4. [Making Changes](#making-changes)
5. [Testing Your Changes](#testing-your-changes)
6. [Submitting Changes](#submitting-changes)
7. [Documentation](#documentation)
8. [Getting Help](#getting-help)

---

## Code of Conduct

Be respectful and constructive. We're all here to make GSD better.

- Be welcoming to newcomers
- Provide helpful feedback
- Focus on the work, not the person
- Accept constructive criticism gracefully

---

## How to Contribute

### Reporting Bugs

1. Check if the bug is already reported in Issues
2. If not, create a new issue with:
   - Clear title describing the problem
   - Steps to reproduce
   - Expected vs actual behavior
   - Your environment (OS, Node version, Claude Code version)

### Suggesting Features

1. Check if the feature is already requested
2. Create an issue describing:
   - The problem you're trying to solve
   - Your proposed solution
   - Alternative approaches you considered

### Submitting Code

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

---

## Development Setup

### Prerequisites

- Node.js 18+
- Git
- Claude Code

### Setup Steps

```bash
# 1. Fork the repository on GitHub

# 2. Clone your fork
git clone https://github.com/YOUR-USERNAME/get-shit-done.git
cd get-shit-done

# 3. Add upstream remote
git remote add upstream https://github.com/original/get-shit-done.git

# 4. Install dependencies
npm install

# 5. Link for development
npm link
```

### Directory Structure

```
get-shit-done/
├── bin/                    # Installation scripts
│   └── install.js
├── commands/               # Slash commands
│   └── gsd/               # All /gsd:* commands
├── get-shit-done/         # Core framework
│   ├── references/        # Reference docs
│   ├── templates/         # File templates
│   └── workflows/         # Execution workflows
├── docs/                  # Documentation
├── .planning/             # Example project state
├── package.json
├── README.md
├── CLAUDE.md
└── CONTRIBUTING.md
```

---

## Making Changes

### Before You Start

1. Check for existing issues/PRs for your change
2. Create an issue if one doesn't exist
3. Wait for feedback before major changes

### Branch Naming

Use descriptive branch names:

- `feature/add-new-command`
- `fix/checkpoint-not-working`
- `docs/improve-user-guide`

### Code Style

- Use consistent indentation (2 spaces)
- Follow existing patterns
- Keep files focused (one purpose per file)
- Write clear comments where needed

### Commit Messages

Format: `type(scope): description`

Types:
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation
- `refactor` - Code refactoring
- `test` - Testing
- `chore` - Maintenance

Examples:
```
feat(commands): add /gsd:my-command
fix(execute): handle checkpoint timeout
docs(readme): add installation section
```

---

## Testing Your Changes

### Manual Testing

1. Create a test project:
   ```bash
   mkdir ~/test-project
   cd ~/test-project
   git init
   ```

2. Start Claude Code and test:
   ```bash
   claude
   # Test your changes
   ```

3. Test common scenarios:
   - Fresh project initialization
   - Mid-project operations
   - Error handling
   - Edge cases

### Testing Checklist

For commands:
- [ ] Command runs without errors
- [ ] Help text is accurate
- [ ] Arguments work correctly
- [ ] Error messages are helpful

For workflows:
- [ ] All steps execute correctly
- [ ] State files are updated properly
- [ ] Git commits are created correctly
- [ ] Checkpoints work as expected

For templates:
- [ ] Files are created with correct structure
- [ ] Placeholders are replaced
- [ ] Formatting is correct

---

## Submitting Changes

### Pull Request Process

1. **Update your fork:**
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Push your branch:**
   ```bash
   git push origin feature/your-feature
   ```

3. **Create Pull Request on GitHub:**
   - Clear title describing the change
   - Reference related issues
   - Describe what you changed and why
   - Include testing notes

4. **Address feedback:**
   - Make requested changes
   - Push updates to the same branch
   - Respond to comments

### PR Requirements

- [ ] Code follows existing style
- [ ] Changes are tested
- [ ] Documentation is updated if needed
- [ ] Commit messages follow conventions
- [ ] No unrelated changes included

---

## Documentation

### When to Update Docs

Update documentation when you:
- Add a new command
- Change command behavior
- Modify file formats
- Add new features
- Fix confusing behavior

### Documentation Files

| File | Update when... |
|------|----------------|
| `README.md` | Major features, commands |
| `docs/USER_GUIDE.md` | User-facing changes |
| `docs/DEVELOPER_GUIDE.md` | Internal changes |
| `docs/QUICK_START.md` | New simple examples |
| `docs/GLOSSARY.md` | New terms, FAQs |
| `CLAUDE.md` | AI-relevant changes |

### Documentation Style

- Use simple, clear language
- Include examples
- Test all commands you document
- Keep formatting consistent

---

## Getting Help

### Questions

- Check existing documentation
- Search closed issues
- Ask in GitHub Discussions

### Stuck on Implementation

- Describe what you're trying to do
- Share what you've tried
- Ask specific questions

### Review Process

- PRs are reviewed by maintainers
- Feedback is provided via comments
- Be patient - reviews take time

---

## Recognition

Contributors are recognized in:
- PR merge messages
- Release notes
- README acknowledgments (for major contributions)

Thank you for contributing to GSD!
