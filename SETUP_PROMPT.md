# Prompt: Set Up Shared AI Agent Guidelines

Use this prompt with any AI coding agent to set up the `.ai/guidelines.md` pattern in a new repository.

**Follow-up**: After running this prompt, use `BMAD_FRAMEWORK_PROMPT.md` to add the full BMAD-lite documentation framework.

---

## The Prompt

````
Set up shared AI agent guidelines for this repository. Follow these steps in exact order.

### Step 1: Analyze the Codebase

Read the codebase and collect:
1. Project overview (what it is, what it does)
2. Repository structure (directories and their purpose — big picture only)
3. Key commands (build, dev, test, lint)
4. Architecture decisions (tech stack, patterns, anything non-obvious)
5. Conventions (naming, file organization, coding style)

### Step 2: Create `.ai/guidelines.md`

Write the information from Step 1 into `.ai/guidelines.md`. This file is the single source of truth for ALL AI coding agents.

Do NOT include:
- Generic development advice ("write tests", "use meaningful names")
- Exhaustive file listings discoverable by browsing
- Information that duplicates the README

### Step 3: Merge Existing Content

IF any of the files listed in Step 4 already exist with real content (not just a pointer):
1. Read their content
2. Merge unique information into `.ai/guidelines.md`
3. Then overwrite them with the pointer in Step 4

IF none exist with real content, skip to Step 4.

### Step 4: Create Pointer Files

Create each file below. Every file uses the **standard pointer** unless marked as an exception.

**Standard pointer** (one line, used by all files except the two exceptions below):
```
Read and strictly follow .ai/guidelines.md for full project guidelines, architecture, commands, and conventions.
```

**Exception A — `CLAUDE.md`** uses this content instead:
```
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Read and strictly follow .ai/guidelines.md for full project guidelines, architecture, commands, and conventions.
```

**Exception B — `.cursor/rules/project.mdc`** uses this content instead:
```
---
description: Project guidelines
globs:
alwaysApply: true
---
Read and strictly follow .ai/guidelines.md for full project guidelines, architecture, commands, and conventions.
```

**File list** (create ALL):
1. `CLAUDE.md` — Claude Code **(use Exception A)**
2. `AGENTS.md` — OpenAI Codex
3. `GEMINI.md` — Google Gemini CLI
4. `.cursorrules` — Cursor (legacy)
5. `.cursor/rules/project.mdc` — Cursor **(use Exception B)**
6. `.github/copilot-instructions.md` — GitHub Copilot
7. `.windsurfrules` — Windsurf
8. `.clinerules` — Cline
9. `.aider.conventions.md` — Aider
10. `.junie/guidelines.md` — JetBrains Junie
11. `.amazonq/rules/guidelines.md` — Amazon Q Developer
12. `.continue/rules.md` — Continue.dev
13. `augment-guidelines.md` — Augment Code
14. `CONVENTIONS.md` — Generic fallback

### Step 5: Verify

Run `git status` and confirm all new files appear as untracked. If any file is missing from the output, it may be git-ignored — warn the user.
````
