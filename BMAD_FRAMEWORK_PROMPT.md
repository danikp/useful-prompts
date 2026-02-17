# Prompt: Set Up BMAD-Lite Documentation Framework

Use this prompt with any AI coding agent to set up a lightweight, BMAD-inspired documentation framework in a repository. Run this **after** `.ai/guidelines.md` exists (see `SETUP_PROMPT.md` to create it first).

---

## The Prompt

```
Analyze this codebase and set up a BMAD-lite documentation framework under `.ai/`.

This framework organizes all project knowledge so any AI agent can pick up work without context loss. It is inspired by the BMAD (Breakthrough Method for Agile AI-Driven Development) framework but right-sized for small-team projects.

Read `.ai/guidelines.md` first to understand the project. Then follow these steps in order:

### 0. Research Phase — Understand the Full Picture

Before creating any documents, gather context from ALL available sources:

**Git history**: Run `git log --oneline -50` and `git log --all --oneline --graph -30` to understand:
- What has been built and in what order
- What phases/milestones have been completed
- Who contributed what (human vs. agent work)
- Any reverts, fixes, or abandoned approaches

For deeper analysis, use `git log --stat -20` to see which files changed in recent commits, and `git log --diff-filter=A --name-only --format=""` to find when files were first added.

**Task management / MCP tools**: Check if any task management MCP servers are available (Atlassian Jira, Asana, Linear, GitHub Projects, Trello, etc.). To check:
- Look at `.mcp.json` in the project root or user home directory
- Check available MCP tool listings
- If a task management tool IS available: read open tasks, sprints, backlogs, and completed items. Use this to populate epics and stories with accurate status. Reference ticket IDs in epic files.
- If NO task management tool is available: rely on git history, TODOs in code, and codebase analysis.

**GitHub/GitLab issues and PRs**: If the repo is hosted on GitHub/GitLab, check for open issues and pull requests using `gh issue list` or `gh pr list` to find planned or in-progress work.

**TODOs in code**: Search for `TODO`, `FIXME`, `HACK`, `XXX` comments in the codebase to find known technical debt and planned improvements.

Then create the following structure:

### 1. Framework README — `.ai/README.md`

Explain the `.ai/` directory structure, what each subdirectory contains, and how agents should use it. Include:
- Directory tree with descriptions
- How to start a new agent session (which files to read)
- How to add a new feature/phase (create epic + prompt)
- When to use which persona

### 2. Project Documentation — `.ai/docs/`

Create these documents by analyzing the actual codebase (don't make up information):

**`.ai/docs/prd.md`** — Product Requirements Document:
- Product overview (what it is, who it's for)
- Target users/personas
- Core features (what exists today)
- Non-functional requirements (performance, scale, browser support)
- Accessibility requirements (WCAG 2.1 AA compliance targets, assistive technology support, keyboard navigation, screen reader compatibility, color contrast ratios)
- Out of scope (what this project deliberately doesn't do)
- Success metrics

**`.ai/docs/architecture.md`** — Technical Architecture:
- System overview diagram (ASCII art showing data flow)
- Component architecture (how parts connect)
- Key technical decisions with rationale
- Scale considerations
- Technology stack with version info

**`.ai/docs/data-dictionary.md`** — Data Schema Reference (if applicable):
- All data models/schemas with TypeScript-style type definitions
- Input/output file descriptions with sizes
- Known data quirks or edge cases
- Relationships between data entities

### 3. Feature Epics — `.ai/epics/`

Create one epic file per major feature area or project phase. Name them `epic-NN-short-name.md`.

For each epic include:
- **Status**: COMPLETED, IN_PROGRESS, or PLANNED
- **Summary**: What this epic delivers
- **Stories/tasks**: Checklist of specific deliverables (checked if done)
- **Key files**: Which files are most relevant
- **Known issues**: Any bugs or limitations
- **Dependencies**: What must be done before/after

Use git history, codebase and task management tools to determine accurate status. Create at least:
- One epic per completed feature area
- One epic for the most obvious next phase of work

### 4. Agent Personas — `.ai/personas/`

Create 3-5 persona files based on the types of work this project needs. Each persona should include:
- **Role**: One-line description
- **Context to load**: Which `.ai/` files to read before starting
- **Principles**: 3-7 rules specific to this role in this project
- **Key commands**: Most-used terminal commands for this work
- **Common tasks**: What this persona typically works on

**IMPORTANT — Always create a Product Owner persona** (`product-owner.md`). This persona is the gateway for all new work. It must include:

- **Role**: Product owner responsible for analyzing requirements, clarifying scope, and writing stories before any implementation begins.
- **Context to load**: PRD, architecture, all epics (to understand current state)
- **Workflow** — the product owner follows this sequence for every new feature or change request:
  1. **Analyze the request** — understand what the user is asking for, identify ambiguities
  2. **Ask clarifying questions** — never assume. Ask the user about scope, priorities, edge cases, acceptance criteria, and trade-offs before proceeding
  3. **Check existing docs** — review PRD, architecture, and epics to see how the request fits with what exists
  4. **Write or update the PRD** — if the request introduces new product requirements, update `.ai/docs/prd.md`
  5. **Write stories** — break the request into concrete, implementable stories with acceptance criteria. Add them to an existing epic or create a new one in `.ai/epics/`
  6. **Write or update the execution prompt** — create a detailed prompt in `.ai/prompts/` that a developer persona can follow to implement the stories
  7. **Hand off** — only after stories are written and approved does work move to a developer persona
- **Principles**:
  - Never skip requirements analysis. "Build X" is not a story — it needs acceptance criteria.
  - Ask questions early. Rework from bad assumptions costs more than a 2-minute clarification.
  - Stories must be testable. Each story should have a clear "done" state.
  - Keep the PRD as the single source of truth for what the product should do.
  - Update docs before handing off — the developer persona should never need to guess intent.

Then create additional personas that match the project's tech stack. Examples:
- `data-engineer.md` — for data pipeline / ETL work
- `frontend-dev.md` — for UI / website work. **Must include an "Accessibility Validation" section** in the persona's workflow with rules covering: semantic HTML (correct elements, no `<div>`/`<span>` for interactive controls), keyboard navigation (Tab/Shift+Tab/Enter/Space/Escape, logical tab order), ARIA attributes (use only when semantic HTML is insufficient — no ARIA is better than bad ARIA), color contrast (WCAG 2.1 AA: 4.5:1 normal text, 3:1 large text), screen reader compatibility (alt text, heading hierarchy, `aria-live` for dynamic content), focus management (modal focus trap, return focus on close, route-change focus), form accessibility (associated `<label>` elements, linked error messages, `aria-required`), and automated checks (eslint-plugin-jsx-a11y, axe-core, Lighthouse, or equivalent). Every frontend story must pass this validation before it can be marked complete.
- `backend-dev.md` — for API / server work
- `reviewer.md` — for code review and QA
- `devops.md` — for CI/CD and deployment
- `mobile-dev.md` — for mobile app work

Only create personas that are relevant to this project's tech stack and workflow.

**On repeat run — persona update rules:**
- **Read every existing persona** before creating or modifying.
- **Update commands**: If the project's build/dev/test commands changed, update the "Key commands" section in each relevant persona.
- **Update principles**: If new architectural decisions were made (visible in architecture.md or git history), add relevant principles.
- **Add personas**: If the project now has a new type of work (e.g., mobile app added, CI/CD pipeline added) not covered by any persona, create a new one.
- **Don't remove personas**: Even if a persona seems unused, keep it — the user may have created it intentionally.
- **Don't overwrite custom content**: If a persona has sections or principles not in the template (user-added), preserve them.

### 5. Execution Prompts — `.ai/prompts/`

If there are any task prompts, build instructions, or phase prompts in the repository (check root for files like `PROMPT_*.md`, `TASK_*.md`, etc.), move them into `.ai/prompts/` and replace the originals with pointer files saying where they moved to.

If the project has obvious future work (from TODOs, issues, or incomplete features), create a prompt for the next logical phase with:
- Context section (what to read first)
- Pre-requisites (what must be done before)
- Numbered task list with acceptance criteria
- Validation steps (how to verify the work is done)

**On repeat run — prompt update rules:**
- **Check root again** for new `PROMPT_*.md` or `TASK_*.md` files that appeared since last run. Move them in.
- **Update existing prompts**: If a prompt's pre-requisites are now met (the phase before it is complete), update the context section to reflect that.
- **Don't overwrite prompts**: Existing prompts may have been hand-edited with specific instructions. Read them fully before modifying. Only update factual references (file paths, command names) that changed.
- **Create next-phase prompt**: If the most recent planned epic now has stories but no corresponding prompt, create one.

### 6. Update `.ai/guidelines.md`

**On first run**: Add a "Documentation Framework" section near the top of `guidelines.md` that lists the key documents, and add the "Agent Workflow Rules" section.

**On repeat run**: Read the existing guidelines.md fully. Only update if:
- New docs/epics/personas/prompts were created (update the document list)
- Commands changed (update the commands sections)
- New conventions were introduced (add to conventions section)
- The "Agent Workflow Rules" section is missing or incomplete (add/fix it)
Do NOT rewrite sections that are already accurate. Do NOT remove custom content the user added.

The "Documentation Framework" section should list the key documents:
- PRD, Architecture, Data Dictionary (under docs/)
- Epics (under epics/)
- Personas (under personas/)
- Prompts (under prompts/)
- Link to `.ai/README.md` for full details

The "Agent Workflow Rules" section should contain these mandatory rules for every agent:

**Communication principle (applies to ALL phases below):**
On every important decision — architectural choices, scope trade-offs, ambiguous requirements, multiple valid approaches — **stop and ask the user before proceeding**. Present the alternatives with pros/cons and a clear recommendation. Never silently pick an option when the choice materially affects the outcome. This applies during requirements analysis, implementation, code review, and any other phase. Silent assumptions are the most expensive kind of rework.

**Before starting work:**
1. Read guidelines (this file)
2. Read the relevant docs (PRD, architecture, data dictionary as needed)
3. Adopt the matching persona from `.ai/personas/`
4. Check epics to understand current status

**For new features or change requests — use the Product Owner persona first:**
5. Adopt `.ai/personas/product-owner.md`
6. Analyze the request, ask clarifying questions
7. Write/update PRD, create stories in epics, write execution prompts
8. Only then switch to a developer persona to implement

**During implementation work:**
9. If task management MCP tools are available (Jira, Asana, Linear, etc.), use them to read assigned tasks and update status
10. Follow the epic's story checklist
11. **Ask before you assume** — when facing important decisions (architectural choices, scope trade-offs, ambiguous requirements, multiple valid approaches), stop and ask the user. Present the options with pros/cons and a recommendation rather than silently picking one.

**Accessibility validation (mandatory for all frontend work):**
12. Follow the accessibility validation rules defined in the relevant frontend persona (e.g., `.ai/personas/frontend-dev.md`). No frontend story is complete until it passes validation.

**Self Code Review (mandatory for all developers before completing work):**
Every developer must perform a self code review before marking any story or task as complete. This catches issues early and ensures alignment with project standards.
13. **Coding standards** — review your changes against the project's conventions (naming, file organization, patterns documented in guidelines). Fix any deviations.
14. **Code quality** — check for dead code, unused imports, console.log statements, hardcoded values, duplicated logic, and overly complex functions. Simplify where possible.
15. **Error handling** — verify that edge cases are handled, error states are surfaced to the user, and no errors are silently swallowed.
16. **Security** — check for injection vulnerabilities (XSS, SQL injection), exposed secrets, and unsafe data handling. Sanitize user input at system boundaries.
17. **Readability** — ensure code is understandable without explanation. If a block of logic is not self-evident, add a brief comment explaining *why* (not *what*).
18. **Diff review** — run `git diff` and read through every changed line as if reviewing someone else's code. Look for accidental changes, leftover debug code, and unintended side effects.

**After completing work:**
19. Update the epic — mark completed stories as `[x]`, change status if all done
20. Update architecture/data-dictionary docs if you made structural changes
21. Update guidelines if you introduced new commands or conventions
22. Log known issues discovered during work to the relevant epic
23. Create follow-up epics if your work revealed new work to be done

These rules ensure documentation stays current and no agent starts from scratch.

### Rules

- **Analyze, don't invent**: Every document should reflect what actually exists in the codebase. Don't add fictional features, fake metrics, or aspirational requirements.
- **Git history is ground truth**: Use `git log` to verify what's been built, what order things happened, and what's complete. Code speaks louder than comments.
- **Cross-reference task management**: If MCP tools for Jira/Asana/Linear/etc. are available, pull real ticket data into epics. If not, reconstruct from git history + code analysis.
- **Be specific**: Reference actual file paths, actual tech stack versions, actual commands.
- **Keep it concise**: Each document should be readable in under 2 minutes. No boilerplate filler.
- **Use checklists**: For epics and stories, use `- [x]` (done) and `- [ ]` (not done) format.
- **TypeScript for schemas**: When documenting data structures, use TypeScript interface notation.
- **Docs are living documents**: The framework prompt creates initial docs, but agents must keep them updated. Stale docs are worse than no docs.
- **Ask questions, propose alternatives**: On every important decision — architecture, scope, trade-offs, ambiguous requirements — ask the user before proceeding. Present options with pros/cons and a recommendation. Silent assumptions are the most expensive kind of rework.
- **Product Owner gates implementation**: No epic should move to IN_PROGRESS without stories and acceptance criteria written by the product owner persona.
- **Accessibility is not optional**: Every frontend story must pass the accessibility validation defined in the frontend persona before it can be marked complete. Accessibility debt is treated the same as a bug — it blocks completion.
- **Self code review before done**: Every developer must review their own changes against coding standards and quality checks before marking work as complete. No story moves to done without a self-review pass.

### 7. Output a Change Summary

After completing all steps, output a summary table:

| File | Action | What Changed |
|------|--------|-------------|
| `.ai/README.md` | Created / Updated / Unchanged | Brief description |
| `.ai/docs/prd.md` | Created / Updated / Unchanged | Brief description |
| `.ai/docs/architecture.md` | Created / Updated / Unchanged | Brief description |
| ... | ... | ... |

This helps the user verify what was done and review only the files that changed.
```
