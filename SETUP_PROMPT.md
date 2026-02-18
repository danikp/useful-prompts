# Prompt: Set Up Shared AI Agent Guidelines

Use this prompt with any AI coding agent to set up the `.ai/guidelines.md` pattern in a new repository.

**Follow-up**: After running this prompt, use `BMAD_FRAMEWORK_PROMPT.md` to add the full BMAD-lite documentation framework (PRD, architecture, epics, personas, execution prompts).

---

## The Prompt

```
Analyze this codebase and create a shared AI agent guidelines file at `.ai/guidelines.md`.

This file should be the single source of truth for ALL AI coding agents working in this repo. Include:

1. Project overview (what it is, what it does)
2. Repository structure (directories and their purpose, focus on the big picture)
3. Key commands (build, dev, test, lint — whatever applies)
4. Architecture decisions (tech stack, patterns, anything non-obvious)
5. Conventions (naming, file organization, coding style, anything specific to this project)

Do NOT include:
- Generic development advice ("write tests", "use meaningful names", etc.)
- Exhaustive file listings that can be discovered by browsing
- Information that duplicates the README

Then create thin pointer files for every major AI coding agent, each containing only a reference to read `.ai/guidelines.md`. Create ALL of the following:

| File | Agent |
|------|-------|
| `CLAUDE.md` | Claude Code — prefix with: `# CLAUDE.md\n\nThis file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.` |
| `AGENTS.md` | OpenAI Codex |
| `GEMINI.md` | Google Gemini CLI |
| `.cursorrules` | Cursor (legacy) |
| `.cursor/rules/project.mdc` | Cursor (new format) — use frontmatter: `---\ndescription: Project guidelines\nglobs:\nalwaysApply: true\n---` |
| `.github/copilot-instructions.md` | GitHub Copilot |
| `.windsurfrules` | Windsurf |
| `.clinerules` | Cline |
| `.aider.conventions.md` | Aider |
| `.junie/guidelines.md` | JetBrains Junie |
| `.amazonq/rules/guidelines.md` | Amazon Q Developer |
| `.continue/rules.md` | Continue.dev |
| `augment-guidelines.md` | Augment Code |
| `CONVENTIONS.md` | Generic fallback |

Each pointer file should contain one line:
"Read and strictly follow .ai/guidelines.md for full project guidelines, architecture, commands, and conventions."

If any of these files already exist with real content, merge their content into `.ai/guidelines.md` first, then replace the original with the pointer.
```
