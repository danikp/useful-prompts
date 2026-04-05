# Prompt: Set Up BMAD-Lite Documentation Framework

Use this prompt with any AI coding agent to set up a BMAD-inspired documentation framework. Run **after** `.ai/guidelines.md` exists (see `SETUP_PROMPT.md`).

**Re-run this prompt** when any of these occur:
- A new major feature is added (3+ new files)
- A dependency is added or removed
- The project structure changes (new directories)
- More than 20 commits since last run

---

## The Prompt

````
Set up a BMAD-lite documentation framework under `.ai/`.

### PREREQUISITE CHECK

Read `.ai/guidelines.md`.
- IF it exists → continue.
- IF it does NOT exist → STOP. Tell the user: "Run SETUP_PROMPT.md first to create .ai/guidelines.md." Do not proceed.

### RUN MODE DETECTION

Check if `.ai/docs/prd.md` exists.
- IF it exists → this is a REPEAT RUN. In each phase, also follow the "REPEAT RUN" instructions.
- IF it does NOT exist → this is a FIRST RUN. Ignore all "REPEAT RUN" blocks.

---

## Named Rules

Define once, referenced by name throughout. Every agent must know these.

**RULE-ASK**: On important decisions (architecture, scope, trade-offs, ambiguous requirements, multiple valid approaches), STOP and ask the user. Present alternatives with pros/cons and a recommendation. Never silently pick an option.

**RULE-A11Y**: Accessibility validation checklist for frontend work:
- Semantic HTML: correct elements; no `<div>`/`<span>` for interactive controls
- Keyboard navigation: Tab/Shift+Tab/Enter/Space/Escape; logical tab order
- ARIA: use only when semantic HTML is insufficient; no ARIA is better than bad ARIA
- Color contrast: WCAG 2.1 AA — 4.5:1 normal text, 3:1 large text
- Screen readers: alt text, heading hierarchy, `aria-live` for dynamic content
- Focus management: modal focus trap, return focus on close, route-change focus
- Forms: associated `<label>`, linked error messages, `aria-required`
- Automated checks: eslint-plugin-jsx-a11y, axe-core, Lighthouse, or equivalent

**RULE-SELF-REVIEW**: Self code review before marking any task complete:
1. Coding standards — check against conventions in guidelines.md. Fix deviations.
2. Code quality — no dead code, unused imports, console.log, hardcoded values, duplication.
3. Error handling — edge cases handled, errors surfaced, none silently swallowed.
4. Security — no injection vulnerabilities, no exposed secrets, input sanitized at boundaries.
5. Readability — code understandable without explanation. Brief *why* comments only where non-obvious.
6. Diff review — run `git diff`, read every line. No accidental changes, debug code, or side effects.

**RULE-PERSONA**: Before starting ANY task, output:
```
PERSONA: [persona name]
CONTEXT LOADED: [list of .ai/ files read]
TASK: [one-line description]
```
If you cannot identify the correct persona, STOP and ask the user.

**RULE-PO-GATE**: Before writing any implementation code, verify ALL:
- [ ] Product Owner persona completed requirements analysis
- [ ] Stories exist in an epic with acceptance criteria
- [ ] Execution prompt exists in `.ai/prompts/`
If any is unchecked, STOP. Switch to Product Owner persona first.

---

## Persona-Task Mapping

| Task type | Required persona | Gate |
|-----------|-----------------|------|
| New feature request | product-owner | RULE-PO-GATE |
| Bug fix (known cause) | matching developer | None |
| Bug fix (unknown cause) | product-owner → developer | PO triages first |
| Code review | reviewer | None |
| Deployment | devops | All tests passing |
| UI/frontend work | frontend-dev | RULE-A11Y required |

---

=== PHASE 1: RESEARCH ===

Gather context from all available sources before creating documents.

1. **Git history**: Run `git log --oneline -50` and `git log --all --oneline --graph -30`.
   For deeper analysis: `git log --stat -20` and `git log --diff-filter=A --name-only --format=""`.
   Note: what was built, build order, completed milestones, reverts, abandoned approaches.

2. **Task management**: Run `ls .mcp.json 2>/dev/null && cat .mcp.json`.
   - IF file exists AND contains task management tools (Jira, Asana, Linear, Trello, GitHub Projects) → read open tasks, sprints, backlogs, completed items.
   - ELSE → skip. Rely on git history, TODOs, and codebase analysis.

3. **GitHub/GitLab issues**: Run `gh issue list` and `gh pr list`. If commands fail, skip.

4. **TODOs in code**: Search for `TODO`, `FIXME`, `HACK`, `XXX` comments.

**Deliverable**: Write findings to `.ai/_research.md` (temporary working file).
**Checkpoint**: List what you found before proceeding.

=== PHASE 1 COMPLETE. Proceed to Phase 2. ===

---

=== PHASE 2: CORE DOCUMENTATION ===

Create these files under `.ai/docs/` by analyzing the actual codebase. Do not invent information.

### 2.1 Framework README — `.ai/README.md`
- Directory tree of `.ai/` with descriptions
- How to start a new agent session (which files to read)
- How to add a new feature (create epic + prompt)
- Persona-Task Mapping reference

### 2.2 Product Requirements — `.ai/docs/prd.md`
- Product overview (what it is, who it's for)
- Target users/personas
- Core features (what exists today)
- Non-functional requirements (performance, scale, browser support)
- Accessibility requirements (reference RULE-A11Y)
- Out of scope (what this project deliberately doesn't do)
- Success metrics

### 2.3 Technical Architecture — `.ai/docs/architecture.md`
- System overview diagram (ASCII art showing data flow)
- Component architecture (how parts connect)
- Key technical decisions with rationale
- Scale considerations
- Technology stack with version info

### 2.4 Data Dictionary — `.ai/docs/data-dictionary.md`
- Create IF the project has database schemas, API request/response types, or structured data files.
- SKIP IF the project has no data persistence or structured I/O.
- Contents: data models with TypeScript-style types, I/O descriptions, data quirks, entity relationships.

**REPEAT RUN**: Read each existing doc. Only update if file paths/commands/versions changed, new features were added, or factual errors exist. Do NOT rewrite accurate sections. Do NOT remove user-added content.

**Deliverable**: Files listed above.
**Checkpoint**: List created/updated files before proceeding.

=== PHASE 2 COMPLETE. Proceed to Phase 3. ===

---

=== PHASE 3: FEATURE EPICS ===

Create one epic per major feature area under `.ai/epics/`. Name: `epic-NN-short-name.md`.

**Feature area** = a distinct group of 3+ related files serving one purpose.

Each epic must include:
- **Status**: `COMPLETED` | `IN_PROGRESS` | `PLANNED`
- **Summary**: What this epic delivers
- **Stories/tasks**: `- [x]` done, `- [ ]` not done. Each story MUST have acceptance criteria.
- **Key files**: Most relevant source files
- **Known issues**: Bugs or limitations
- **Dependencies**: What must be done before/after

Minimum required:
- One epic per completed feature area
- One epic for the most obvious next phase of work

Use git history and `.ai/_research.md` data to determine accurate status.

**REPEAT RUN**: Read all existing epics first. Update story checklists to match implementation state (verify via git log). Add new epics only for uncovered areas. Do NOT remove or overwrite existing epics.

**Deliverable**: `.ai/epics/epic-NN-*.md` files.
**Checkpoint**: List all epics with status before proceeding.

=== PHASE 3 COMPLETE. Proceed to Phase 4. ===

---

=== PHASE 4: AGENT PERSONAS ===

Create one persona per distinct role under `.ai/personas/`.
Minimum: Product Owner + one developer persona. Maximum: 6.
Choose roles based on languages and frameworks in the tech stack.

Each persona file must include:
- **Role**: One-line description
- **Context to load**: Which `.ai/` files to read before starting
- **Principles**: 3-7 rules specific to this role in this project
- **Key commands**: Most-used terminal commands
- **Common tasks**: What this persona typically works on

### 4.1 MANDATORY — Product Owner (`product-owner.md`)

This persona gates all new work (see RULE-PO-GATE).

- **Role**: Analyzes requirements, clarifies scope, writes stories before implementation.
- **Context to load**: PRD, architecture, all epics.
- **Workflow** (for every new feature or change request):
  1. Analyze the request — identify ambiguities
  2. Ask clarifying questions (follow RULE-ASK) — scope, priorities, edge cases, acceptance criteria
  3. Check existing docs — PRD, architecture, epics for fit
  4. Update PRD — if new product requirements are introduced
  5. Write stories — concrete, testable, with acceptance criteria. Add to existing or new epic.
  6. Write execution prompt in `.ai/prompts/` for a developer persona
  7. Hand off — work moves to developer only after stories are written and approved
- **Principles**:
  - "Build X" is not a story. Every story needs acceptance criteria.
  - Ask questions early. Bad assumptions cause expensive rework.
  - Stories must be testable with a clear "done" state.
  - PRD is the single source of truth for product requirements.
  - Update docs before handing off. Developers must never guess intent.

### 4.2 Developer Personas

Create personas matching the tech stack. Options (only create what's relevant):
- `frontend-dev.md` — UI/website. MUST include RULE-A11Y as a mandatory workflow section.
- `backend-dev.md` — API/server work
- `data-engineer.md` — data pipeline/ETL
- `reviewer.md` — code review and QA
- `devops.md` — CI/CD and deployment
- `mobile-dev.md` — mobile app work

**REPEAT RUN**: Read every existing persona first. Update "Key commands" if commands changed. Update "Principles" if new architectural decisions exist. Add new personas for uncovered work types. Do NOT remove personas. Do NOT overwrite user-added content.

**Deliverable**: `.ai/personas/*.md` files.
**Checkpoint**: List personas created before proceeding.

=== PHASE 4 COMPLETE. Proceed to Phase 5. ===

---

=== PHASE 5: PROMPTS & GUIDELINES UPDATE ===

### 5.1 Execution Prompts — `.ai/prompts/`

IF prompt files exist in repo root (`PROMPT_*.md`, `TASK_*.md`):
- Move them to `.ai/prompts/`
- Replace originals with: "Moved to `.ai/prompts/[filename]`"

IF the project has obvious future work (TODOs, issues, incomplete features):
- Create a prompt for the next logical phase:
  - Context section (what to read first)
  - Pre-requisites (what must be done before)
  - Numbered task list with acceptance criteria
  - Validation steps

**REPEAT RUN**: Check root for new prompt files and move them. Update existing prompts only if factual references changed. Do NOT overwrite hand-edited prompts. Create next-phase prompt if newest planned epic has stories but no prompt.

### 5.2 Update `.ai/agent-workflow.md`

Create or update `.ai/agent-workflow.md` with the full agent session workflow. This file is separate from `guidelines.md` to prevent guidelines bloat — `guidelines.md` stays lean as a routing table, while workflow details live here.

**Content for `agent-workflow.md`:**

```
# Agent Workflow Rules

## Communication Principle
On every important decision — architectural choices, scope trade-offs, ambiguous requirements, multiple valid approaches — stop and ask the user. Present alternatives with pros/cons. Never silently pick an option. (RULE-ASK)

## Before starting work:
1. Read guidelines.md — critical rules and routing table
2. Read conventions.md — if you will write or modify code
3. Read relevant docs from the routing table based on your task
4. Follow RULE-PERSONA — adopt a persona before any task
5. Check epics to understand current status

## For new features or change requests:
6. Follow RULE-PO-GATE — Product Owner must complete analysis first
7. After PO hand-off, switch to developer persona to implement

## During implementation:
8. Use task management MCP tools if available to read tasks and update status
9. Follow the epic's story checklist
10. Follow RULE-ASK on every important decision

## Frontend work:
11. Follow RULE-A11Y. No frontend story is complete without passing it.

## Before marking any task complete:
12. Follow RULE-SELF-REVIEW.

## After completing work:
13. Update epic — mark completed stories [x], update status
14. Update architecture/data-dictionary if structural changes were made
15. Update conventions if new commands or conventions introduced
16. Log known issues to relevant epic
17. Create follow-up epics if new work was discovered
```

### 5.3 Update `.ai/guidelines.md`

Update the routing table in `guidelines.md` to include all new files created by this framework. Add entries for:
- `agent-workflow.md` — Starting a new session or task
- `docs/prd.md` — When you need product context or feature scope
- `docs/architecture.md` — Working across apps/libs or adding services
- `docs/data-dictionary.md` — Working with entities, queries, or migrations
- `epics/epic-*.md` — Always find the epic matching your feature area
- `personas/*.md` — Always adopt the persona matching your task
- `prompts/*.md` — Before starting a major task, check for an existing prompt

IF `guidelines.md` has a "Domain Critical Rules" section, add 1-2 line rules for any new domain docs created.

**IMPORTANT: Do NOT add detailed workflow rules, conventions, or domain rules to `guidelines.md`.** That file must stay under ~100 lines as a lean entry point. Detailed content belongs in topic files.

**REPEAT RUN**: Read existing files fully. Only update outdated or incomplete sections. Do NOT rewrite accurate sections. Do NOT remove user-added content.

**Deliverable**: Updated prompt files, agent-workflow.md, and guidelines.md routing table.
**Checkpoint**: List changes made.

=== PHASE 5 COMPLETE. Proceed to Phase 6. ===

---

=== PHASE 6: SELF-IMPROVEMENT AUDIT (mandatory on every run) ===

### 6.1 Drift Detection
For each document in `.ai/docs/` and `.ai/epics/`:
- Compare claims against actual codebase state
- List inconsistencies (outdated paths, missing features, wrong versions)
- Rate: `NONE` | `LOW` (cosmetic) | `HIGH` (factual error)

### 6.2 Completeness Check
- [ ] Every source directory mentioned in architecture.md
- [ ] Every dependency (package.json, requirements.txt, go.mod, etc.) in tech stack
- [ ] Every epic's checklist matches implementation state (verify via git log)
- [ ] Every persona's "Key commands" matches available commands
- [ ] Named rules (RULE-ASK, RULE-A11Y, RULE-SELF-REVIEW, RULE-PERSONA, RULE-PO-GATE) referenced in agent-workflow.md
- [ ] guidelines.md routing table includes all `.ai/docs/` and `.ai/epics/` files
- [ ] guidelines.md is under ~100 lines (lean router, not a reference manual)
- [ ] No single `.ai/` file exceeds ~500 lines

### 6.3 Prompt Effectiveness Review
For each `.ai/prompts/*.md`:
- Flag prompts referencing nonexistent files, epics, or personas
- Flag prompts with pre-requisites now met (update their status)

### 6.4 Apply Fixes
- Fix HIGH drift immediately
- Fix LOW drift if under 5 lines
- For structural issues (missing epics, wrong architecture), follow RULE-ASK first

### 6.5 Log Changes
Append to `.ai/CHANGELOG.md`:
```
## [YYYY-MM-DD]
- [file]: [what changed and why]
```
IF `.ai/CHANGELOG.md` does not exist, create it.

### 6.6 Cleanup
Delete `.ai/_research.md` (temporary file from Phase 1).

=== PHASE 6 COMPLETE. Proceed to Phase 7. ===

---

=== PHASE 7: SUMMARY ===

Output a change summary table:

| File | Action | What Changed |
|------|--------|-------------|
| `.ai/README.md` | Created / Updated / Unchanged | Brief description |
| `.ai/docs/prd.md` | Created / Updated / Unchanged | Brief description |
| `.ai/docs/architecture.md` | Created / Updated / Unchanged | Brief description |
| ... | ... | ... |

=== ALL PHASES COMPLETE ===

---

## Rules

- **Analyze, don't invent**: Every document must reflect what actually exists. No fictional features or aspirational requirements.
- **Git history is ground truth**: Use `git log` to verify what's built and complete.
- **Cross-reference task management**: Use MCP tool data if available. Otherwise reconstruct from git + code.
- **Be specific**: Actual file paths, actual versions, actual commands.
- **Maximum 200 lines per document** (epics, personas, prompts). Technical reference docs may be longer but should not exceed ~500 lines.
- **Prevent guidelines bloat**: `guidelines.md` is a lean router (~60-100 lines) with critical rules and a routing table. Detailed content goes in topic files (`conventions.md`, `agent-workflow.md`, `docs/*.md`). If a topic grows large, split it. Each topic file must have its own "Critical Rules" section at the top.
- **Use checklists**: `- [x]` done, `- [ ]` not done.
- **TypeScript for schemas**: Use TypeScript interface notation for data structures.
- **Product Owner gates implementation**: RULE-PO-GATE is mandatory.
- **Accessibility is not optional**: Frontend stories must pass RULE-A11Y.
- **Self-review before done**: RULE-SELF-REVIEW is mandatory for all developers.
````
