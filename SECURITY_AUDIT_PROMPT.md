# Prompt: Security Audit & Remediation System

Use this prompt with any AI coding agent to establish a security audit workflow, triage findings, fix issues using TDD, and embed ongoing security checks into the project. Works with any tech stack.

This is a 4-phase system — run each phase in sequence:

1. **Audit** — find vulnerabilities
2. **Triage** — decide what to fix vs accept
3. **Fix** — remediate issues using TDD
4. **Embed** — add security rules to project guidelines

---

## Phase 1: Security Audit

````
Run a security audit on this codebase. This is a research-only task — do NOT modify any code.

### Step 1: Understand the Project

Read the project structure, entry points, configuration files, and any existing documentation or guidelines. Identify:
- What the application does (web API, CLI, library, etc.)
- Tech stack (language, framework, ORM, auth mechanism, deployment)
- Entry points (routes, controllers, handlers, CLI commands)
- Data stores (database, cache, file storage, message queues)
- External integrations (third-party APIs, OAuth providers, webhooks)
- Multi-tenancy model (if any) — how are tenants isolated?

### Step 2: Run 5 Parallel Investigation Tracks

Investigate all source files (excluding dependencies/vendor) across these tracks:

#### Track 1: Authentication & Access Control
Look for:
- Endpoints/routes missing authentication checks
- JWT/session configuration issues (weak secrets, missing algorithm lock, no expiry)
- Password handling (weak hashing, missing complexity rules)
- Missing rate limiting on auth endpoints (login, password reset, OTP)
- Auth bypass mechanisms (master passwords, debug backdoors)
- Permission/role checks missing on protected endpoints
- OAuth/SSO redirect URL validation — are redirect targets validated against an allowlist?

#### Track 2: Injection & Input Validation
Search for string interpolation in:
- SQL queries (including ORM raw queries, JSONB operators, WHERE clauses)
- Shell commands (exec, spawn, system calls)
- Template engines (Handlebars, Jinja, EJS, etc.) with user-controlled templates
- LDAP queries, regex from user input, file path construction

Also check:
- Missing input validation on API endpoints
- Endpoints accepting untyped/dynamic objects without schema validation
- File upload handling (type validation, size limits, path traversal)

#### Track 3: Secrets & Configuration
Search all source files for:
- Hardcoded credentials (passwords, API keys, tokens, connection strings)
- Secrets in default/fallback values
- .env files committed to git (check .gitignore)
- Sensitive data in log statements (passwords, tokens, OTP codes, PII)
- Console/debug logging in production code (bypasses structured logging)
- Security headers (X-Frame-Options, X-Content-Type-Options, HSTS, CSP, Referrer-Policy)
- CORS configuration (open/permissive origins)
- Error responses leaking stack traces or internal details

#### Track 4: Data Isolation & Authorization
Look for:
- Database queries missing tenant/org scoping (if multi-tenant)
- Cache keys missing tenant identifiers (cross-tenant cache pollution)
- Bulk/batch operations not scoped to authorized tenant
- Background jobs that lose tenant context
- API endpoints vulnerable to IDOR (accessing other users' data by ID)
- Missing ownership checks on update/delete operations

#### Track 5: Infrastructure & Dependencies
Check:
- Dockerfile security (non-root user, minimal image, no baked-in secrets, HEALTHCHECK)
- Container orchestration (security contexts, health probes, network policies, resource limits)
- Database connection SSL configuration
- External HTTP calls without timeouts
- Message queue TLS enforcement
- Dependency vulnerabilities (run audit command for your package manager)
- Custom cryptography implementations (should use standard libraries)

### Step 3: Write the Report

Create the report at `.ai/audits/SECURITY-REVIEW-<YYYY-MM-DD>.md`:

```
# Security Review Report

**Date:** <YYYY-MM-DD>
**Scope:** Full codebase security audit

## Executive Summary
<2-3 sentences. Total findings by severity.>

| Severity | Count |
|----------|-------|
| CRITICAL | N |
| HIGH     | N |
| MEDIUM   | N |
| LOW      | N |

## CRITICAL FINDINGS
### C1. <Title>
- **File:** `<path>:<line>`
- **Issue:** <description with code snippet>
- **Impact:** <what an attacker could do>
- **Remediation:** <specific fix>

## HIGH FINDINGS
### H1. ...

## MEDIUM FINDINGS
### M1. ...

## LOW FINDINGS
### L1. ...

## Positive Findings
<Security practices done well>

## Remediation Priority
### Phase 1 - Immediate (This Week): CRITICAL items
### Phase 2 - Sprint Priority (2 Weeks): HIGH items
### Phase 3 - Scheduled (Next Month): MEDIUM items
### Phase 4 - Ongoing: LOW items
```

### Severity Classification

| Severity | Criteria |
|----------|----------|
| CRITICAL | Exploitable now with direct data breach, credential exposure, or unauthorized access. |
| HIGH | Significant gap requiring specific conditions to exploit, but serious impact. |
| MEDIUM | Defense-in-depth weakness or missing best practice that increases attack surface. |
| LOW | Minor hardening opportunity or documentation issue. |
````

---

## Phase 2: Triage Findings

Run this after the audit is complete.

````
Triage the security audit findings. Read the audit report at `.ai/audits/SECURITY-REVIEW-<YYYY-MM-DD>.md`.

### Step 1: Create Triage Decisions File

Create `.ai/audits/security-triage-decisions.md` (or update if it exists):

```
# Security Audit Triage Decisions

> **Purpose:** Persistent record of security triage decisions. Issues marked
> `ACCEPTED_RISK` should NOT be re-raised in future audits unless their
> `Conditions to Re-evaluate` are met.

## ACCEPTED RISKS

### <ID>. <Title>
- **Decision:** ACCEPTED_RISK
- **Date:** <YYYY-MM-DD>
- **Finding:** <one-line summary>
- **Rationale:** <why the risk is acceptable>
- **Conditions to Re-evaluate:** <when this decision should be revisited>

## ITEMS TO HANDLE

| ID | Finding | Severity | Status |
|----|---------|----------|--------|
| C1 | <title> | CRITICAL | PENDING |
| H1 | <title> | HIGH     | PENDING |
```

### Step 2: Present Each Finding for Triage

For each finding from the audit, present to the user:
- The finding ID, title, severity, and file location
- The potential impact and remediation effort
- Your recommendation: HANDLE or ACCEPTED_RISK with rationale

**Ask the user for their decision on each finding.** Record all decisions in the triage file.

### Rules:
- The user's decision is final — record it even if you disagree.
- ACCEPTED_RISK requires a rationale and re-evaluation conditions.
- Group related findings if they clearly share the same decision (e.g., 12 instances of the same enum interpolation pattern).
- HANDLE items start with status PENDING.
````

---

## Phase 3: Fix Security Issues (TDD)

Run this after triage is complete, as many times as needed.

````
Fix PENDING security issues from `.ai/audits/security-triage-decisions.md` using a test-driven approach.

### Step 1: Load Context

1. Read `.ai/audits/security-triage-decisions.md` — focus on the ITEMS TO HANDLE table.
2. Filter to only PENDING items.
3. Read project guidelines/conventions if they exist.

### Step 2: Group Related Issues

Group PENDING issues by relatedness — same root cause, same file area, or same fix pattern. Examples:
- SQL injection in multiple query files → single group
- Missing tenant scoping in multiple repositories → single group
- Hardcoded credentials across services → single group

Present groups to the user with:
- Group name and issue IDs
- Severity levels
- Brief description of what the fix involves
- Estimated blast radius (how many files touched)

**Ask the user which group to fix.**

### Step 3: Research

For each issue in the selected group:
1. Read the vulnerable code and its surrounding context
2. Find existing patterns in the codebase that show the correct/safe approach
3. Identify all callers/consumers that will be affected by the fix
4. Note any existing tests for the affected code

### Step 4: Write Failing Tests First (TDD)

Write tests that demonstrate the vulnerability or verify the fixed behavior:

**Injection vulnerabilities (SQL, command, template):**
- Pass malicious input that would cause an error or unexpected behavior
- Assert the method handles it gracefully (parameterized, escaped, or rejected)

**Missing authorization/tenant isolation:**
- Test cross-tenant access — request data belonging to Tenant B while authenticated as Tenant A
- Assert the request is rejected or returns empty results

**Hardcoded credentials:**
- Source-scanning tests: read the source file and assert no credential-like patterns exist
- Assert environment variable / config reading pattern is used

**Logging sensitive data:**
- Source-scanning tests: read the source file and assert no secrets/codes/PII in log calls

**Configuration issues (headers, JWT, redirects):**
- Unit test the specific configuration
- Test both allowed and blocked/malicious cases

Run tests to confirm they FAIL with the current (vulnerable) code.

### Step 5: Apply the Fix

Apply the minimal fix. Follow these principles:
- **Use existing codebase patterns.** Find a safe example and follow it.
- **Don't over-engineer.** Fix the specific vulnerability, don't refactor surrounding code.
- **Ask on important decisions.** E.g., "Should the API URL also be an env var or is it fine as a constant?"
- **Don't add new dependencies** unless explicitly approved.
- **Consider dev/local environments.** Some fixes (like HSTS) should skip local/test.
- **Check all callers.** When changing a function signature, find and update ALL call sites.
- **Verify compilation/lint passes** after each fix.

### Step 6: Validate

Run the tests from Step 4. All should now PASS.
Also run existing tests for affected code to ensure no regressions.

### Step 7: Update Triage

In `.ai/audits/security-triage-decisions.md`, update each fixed issue:
```
| ID | Finding | Severity | RESOLVED (<date>) — <brief description of fix> |
```

### Step 8: Continue or Stop

Present remaining PENDING groups and ask which to fix next.

### Important Reminders

- **Never skip the test step.** Even simple fixes benefit from regression tests.
- **Rotate exposed credentials.** When moving hardcoded secrets to env vars, remind the user to rotate the values since they're in git history.
- **Future audits respect triage.** Issues marked ACCEPTED_RISK won't be re-raised unless re-evaluation conditions are met.
````

---

## Phase 4: Embed Security Rules in Project

Run this after fixing issues to prevent regressions.

````
Based on the security audit findings and fixes applied, add project-specific security rules to the project's guidelines and review checklists. These rules should be checked on every code change going forward.

### Step 1: Identify Rules from Findings

Review the triage decisions file (`.ai/audits/security-triage-decisions.md`) — both RESOLVED items and ACCEPTED_RISK items. Extract recurring patterns into concrete, actionable rules.

Common rule categories (include only those relevant to this project):

**Credentials & Secrets:**
- No hardcoded credentials in source code — use environment variables / secret management
- Never commit .env files — add to .gitignore
- API URLs that are not environment-dependent may remain as code constants

**Injection Prevention:**
- All database queries must use parameterized queries / parameter binding — never string interpolation with user input
- Shell commands must not include user input, or must use safe APIs (no string interpolation)
- Template engines must use escaped output for user data

**Authentication & Authorization:**
- JWT signing must lock the algorithm (e.g., HS256) on both sign and verify
- User-controlled redirect URLs must be validated against an allowlist
- Every protected endpoint must have auth guard / middleware coverage

**Data Isolation (if multi-tenant):**
- Every database query must include tenant scoping (e.g., companyId, orgId, teamId)
- Cache keys for tenant-scoped data must include the tenant identifier
- Background jobs must propagate tenant context

**Logging & Data Exposure:**
- Never log secrets, passwords, OTP codes, tokens, or PII
- Use the project's structured logger — no direct console output in production code
- API error responses must not expose stack traces or internal details

**HTTP & Infrastructure:**
- Security headers must be set (X-Content-Type-Options, X-Frame-Options, etc.)
- HSTS should only be enforced in production (not local dev)
- External HTTP calls must include a timeout
- Dockerfile should use non-root user

### Step 2: Add Security Rules Section to Guidelines

If the project has an AI guidelines file (e.g., `.ai/guidelines.md`, `CLAUDE.md`, `AGENTS.md`), add a **Security Rules** section with the identified rules. Also add a non-negotiable critical rule referencing the security checklist.

### Step 3: Add Security Checklist to Self-Review / Pre-Commit

Add a **Security Checklist** to the project's self-review or pre-commit checklist (if one exists). This is a quick list of items every developer (human or AI) should verify before completing any task:

```
#### Security Checklist (run for every code change)
- [ ] No hardcoded credentials, API keys, tokens, or passwords in source code
- [ ] All database queries use parameter binding — no string interpolation with user input
- [ ] All new queries include tenant scoping (if multi-tenant)
- [ ] Cache keys include tenant identifier for tenant-scoped data (if multi-tenant)
- [ ] No secrets, OTP codes, or PII in log statements
- [ ] Using project's structured logger — no console.error/console.warn in production
- [ ] User-controlled redirect URLs validated against allowlist
- [ ] JWT sign/verify locks algorithm explicitly
- [ ] External HTTP calls include timeout
- [ ] Template engines use escaped output for user data
```

Adapt this checklist to the project's specific tech stack and patterns.

### Step 4: Update Code Review Process

If the project has a code review prompt, checklist, or reviewer persona, add security as a review criterion with the same checklist items.

### Step 5: Reference the Audit

Add a note in the guidelines pointing to `.ai/audits/security-triage-decisions.md` as the authoritative record of security decisions, so future audits and reviews can reference it.
````

---

## Running Future Audits

After the initial setup, subsequent audits should:

1. Read `.ai/audits/security-triage-decisions.md` before starting
2. **Skip** `ACCEPTED_RISK` items unless their `Conditions to Re-evaluate` have been met
3. Carry forward `PENDING` items that are still unfixed
4. Mark `PENDING` items as `RESOLVED` if they've been fixed since the last audit
5. Compare findings against the previous audit report to track progress and detect regressions
