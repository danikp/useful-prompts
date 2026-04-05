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

### Step 2: Create the Guidelines Files

Write the information from Step 1 into **three files** under `.ai/`. This multi-file structure prevents guidelines bloat — AI agents have token limits per file read, so keeping files focused and under ~500 lines each ensures they can always be read in full.

#### 2.1 `.ai/guidelines.md` — Entry Point & Critical Rules

This is the **entry point** that all AI agent pointer files reference. Keep it lean (~60-100 lines). It contains:

1. **Critical Rules** — Non-negotiable rules that apply to every task (e.g., "always filter by tenantId", "use X error handling pattern"). These are the rules so important that every agent must see them on every session, without needing to read additional files.
2. **Domain Critical Rules** — 1-2 line summaries of key rules from each domain area. Enough for an agent to decide "do I need to read the full doc?" without loading it.
3. **File Routing Table** — A table mapping each `.ai/` file to a **trigger** (when to read it). Format:

| File | Trigger — Read When... |
|------|------------------------|
| `conventions.md` | Writing or modifying any code |
| `agent-workflow.md` | Starting a new session or task |
| `docs/architecture.md` | Working across apps/libs or adding services |
| ... | ... |

Do NOT put conventions, workflow rules, or detailed domain rules in this file — those go in the files below.

#### 2.2 `.ai/conventions.md` — Coding Conventions & Commands

All coding conventions, key commands, and patterns (~150-250 lines):
- Key commands table (build, test, lint, etc.)
- Error handling patterns
- File organization rules
- Naming conventions, typing rules, coding style
- SQL/database patterns
- Formatting configuration
- Any other project-specific coding patterns

#### 2.3 `.ai/agent-workflow.md` — Agent Session Workflow

How AI agents should approach work (~50-100 lines, will grow with BMAD framework):
- Communication principle (when to ask vs. decide)
- What to read before starting
- Self-review checklist before completing work
- Any project-specific workflow rules

### Guidelines Bloat Prevention

The multi-file structure exists to prevent a common problem: guidelines growing into a single massive file that exceeds agent token limits. Follow these rules:

- **`guidelines.md` must stay under 100 lines.** It is a router, not a reference manual. If you need to add content, put it in the appropriate topic file and add a routing table entry.
- **No file should exceed ~500 lines.** If a topic file grows beyond this, split it into subtopics.
- **Critical rules go in `guidelines.md`; details go in topic files.** A critical rule is one that applies to nearly every task and is short enough to include without explanation. Detailed explanations, examples, and edge cases belong in the relevant topic file.
- **Each topic file must have its own "Critical Rules" section at the top.** When an agent follows a routing table link, the first thing it reads should be the most important rules for that topic — not a table of contents or history section.
- **Use triggers, not "read everything."** The routing table tells agents which files to read based on their task, reducing unnecessary context loading.

Do NOT include in any guidelines file:
- Generic development advice ("write tests", "use meaningful names")
- Exhaustive file listings discoverable by browsing
- Information that duplicates the README

### Step 3: Merge Existing Content

IF any of the files listed in Step 4 already exist with real content (not just a pointer):
1. Read their content
2. Merge unique information into the appropriate `.ai/` file (guidelines, conventions, or agent-workflow)
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
