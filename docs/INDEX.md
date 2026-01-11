# Documentation Index - Get Shit Done (GSD)

Welcome to the GSD documentation! This index will help you find what you need.

---

## For Users (Non-Technical)

Start here if you want to use GSD to build projects with AI assistance.

### Getting Started Path

1. **[Installation Guide](INSTALLATION.md)** - Set up everything you need
2. **[Quick Start Guide](QUICK_START.md)** - Get running in 5 minutes with 10 examples
3. **[User Guide](USER_GUIDE.md)** - Complete reference for daily use

### Quick Links

| I want to... | Read this |
|--------------|-----------|
| Install GSD | [Installation Guide](INSTALLATION.md) |
| Start my first project | [Quick Start - Use Case 1](QUICK_START.md#use-case-1-personal-notes-app) |
| Understand commands | [User Guide - Commands](USER_GUIDE.md#all-commands-explained) |
| Learn what terms mean | [Glossary](GLOSSARY.md#glossary-of-terms) |
| Fix a problem | [Glossary - FAQ](GLOSSARY.md#frequently-asked-questions) |

---

## For Developers

Start here if you want to modify GSD, add features, or understand internals.

### Getting Started Path

1. **[Developer Guide](DEVELOPER_GUIDE.md)** - Complete guide to GSD internals
2. **[Architecture Guide](ARCHITECTURE.md)** - System design and data flow
3. **[CLAUDE.md](../CLAUDE.md)** - Quick reference for AI assistance

### Quick Links

| I want to... | Read this |
|--------------|-----------|
| Set up development environment | [Developer Guide - Setup](DEVELOPER_GUIDE.md#setting-up-your-development-environment) |
| Understand the architecture | [Architecture Guide](ARCHITECTURE.md) |
| Add a new command | [Developer Guide - New Commands](DEVELOPER_GUIDE.md#how-commands-work) |
| Modify a workflow | [Developer Guide - Workflows](DEVELOPER_GUIDE.md#how-workflows-work) |
| Debug issues | [Developer Guide - Debugging](DEVELOPER_GUIDE.md#debugging-guide) |

---

## Document Overview

### User Documentation

| Document | Description | Audience |
|----------|-------------|----------|
| [INSTALLATION.md](INSTALLATION.md) | Step-by-step installation for all platforms | Everyone |
| [QUICK_START.md](QUICK_START.md) | 5-minute start + 10 educational examples | New users |
| [USER_GUIDE.md](USER_GUIDE.md) | Complete user manual | All users |
| [GLOSSARY.md](GLOSSARY.md) | Terms, FAQ, and quick reference | All users |

### Developer Documentation

| Document | Description | Audience |
|----------|-------------|----------|
| [DEVELOPER_GUIDE.md](DEVELOPER_GUIDE.md) | How GSD works internally | Contributors |
| [ARCHITECTURE.md](ARCHITECTURE.md) | System design and patterns | Contributors |
| [CLAUDE.md](../CLAUDE.md) | AI assistant context | Claude Code |

---

## Learning Paths

### Path 1: "I just want to build something"

Time: 30 minutes

1. [Installation Guide](INSTALLATION.md) - 10 min
2. [Quick Start - First 3 Use Cases](QUICK_START.md#use-case-1-personal-notes-app) - 20 min

### Path 2: "I want to master GSD"

Time: 2-3 hours

1. [Installation Guide](INSTALLATION.md) - 10 min
2. [Quick Start - All 10 Use Cases](QUICK_START.md) - 60 min
3. [User Guide - Complete](USER_GUIDE.md) - 60 min
4. [Glossary](GLOSSARY.md) - 30 min

### Path 3: "I want to contribute to GSD"

Time: 4-5 hours

1. [Installation Guide](INSTALLATION.md) - 10 min
2. [Quick Start - All Use Cases](QUICK_START.md) - 60 min
3. [Developer Guide - Complete](DEVELOPER_GUIDE.md) - 90 min
4. [Architecture Guide](ARCHITECTURE.md) - 60 min
5. Practice: Make a small change - 60 min

---

## Document Conventions

### Formatting

Throughout the documentation:

- `monospace` - Commands, file names, code
- **bold** - Important terms, UI elements
- *italic* - Emphasis, new concepts
- > Quotes - Tips and notes

### Command Examples

Commands are shown in code blocks:

```bash
/gsd:command-name argument
```

### File Paths

File paths use forward slashes for consistency:

- `~/.claude/get-shit-done/` - GSD installation
- `.planning/` - Project planning directory

### Screenshots and Diagrams

Text-based diagrams are used for compatibility:

```
┌─────────────┐
│   Box 1     │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Box 2     │
└─────────────┘
```

---

## Getting Help

### In This Documentation

1. Use `Ctrl+F` (or `Cmd+F`) to search within pages
2. Check the [Glossary](GLOSSARY.md) for term definitions
3. See [FAQ](GLOSSARY.md#frequently-asked-questions) for common questions

### In Claude Code

1. Run `/gsd:help` for command list
2. Ask Claude directly: "How do I..."
3. Run `/gsd:progress` to see current state

### Community

1. GitHub Issues for bugs
2. GitHub Discussions for questions
3. Pull Requests for contributions

---

## Contributing to Documentation

Found an error or want to improve the docs?

1. Fork the repository
2. Edit the relevant `.md` file in `docs/`
3. Submit a pull request

Guidelines:
- Keep language simple and clear
- Include examples where helpful
- Test any commands you document
- Follow existing formatting conventions

---

## Changelog

### Version 1.0 (Current)

- Initial documentation release
- Installation Guide for all platforms
- Quick Start with 10 use cases
- Complete User Guide
- Developer Guide with internals
- Architecture documentation
- Glossary and FAQ

---

Happy building with GSD!
