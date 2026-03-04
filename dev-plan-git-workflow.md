# Codex Execution Rules (PLAN.md–Driven Workflow)

---

# 0. PLAN.md as Single Source of Truth

- All implementation scope must strictly follow `PLAN.md`.
- No feature, refactor, or structural change may be implemented unless defined in `PLAN.md`.
- In case of ambiguity, prioritize these sections in `PLAN.md`:
  - Confirmed Decisions
  - Scope (In / Out)
  - Public API / Types
  - Acceptance Criteria
  - Implementation Steps
- If changes are required outside the current PLAN definition:
  1. Update `PLAN.md` first
  2. Get confirmation
  3. Then implement

---

# HARD STOP RULES (Mandatory)

These rules override all other instructions.

If any of the following conditions occur, **STOP implementation immediately**.

## Hard Stop Conditions

1. Implementation requires changes not defined in `PLAN.md`
2. Multiple implementation steps are attempted at once
3. Behavior contradicts `Scope (Out)` definition
4. API / Type definitions differ from `Public API / Types`
5. Directory structure conflicts with `Directory / Module Design`

## Hard Stop Action

When a violation occurs:

1. Stop implementation immediately
2. Explain the conflict
3. Propose the required `PLAN.md` modification
4. Wait for confirmation
5. Only resume work after PLAN is updated

Never bypass this rule.

---

# 1. Work Start Procedure (Mandatory)

Before starting any work:

1. Identify the current step from **Implementation Steps (Execution Order)** in `PLAN.md`.
2. Select exactly **one step** to implement.
3. Review only the sections relevant to that step:
   - Routing / structure → `Directory / Module Design`
   - Types / API responses → `Public API / Types`
   - Policies / constraints → `Metric Aggregation Design`, `Scope`
   - UX → `Page Behavior`, `UI / Design System`
4. Do NOT implement anything listed under **Out (v1 excluded)**.
5. If deviation from PLAN is necessary:
   - Modify `PLAN.md` first
   - Wait for confirmation
   - Then proceed

---

# 2. Branch Rules (Meaning-Based)

## 2.1 Branch Creation

- Create a new branch at the start of each implementation step.
- Branch names must describe the feature or change.
- Do NOT include step numbers in branch names.

Naming convention:

```
feat/<feature-slug>
fix/<feature-slug>
refactor/<feature-slug>
chore/<feature-slug>
```

Examples:

```
feat/nextjs-init
feat/blog-layout
feat/redis-metrics
feat/infinite-scroll
fix/ip-dedupe-logic
refactor/content-loader
```

## 2.2 Branch Principles

- One branch = One implementation step
- Branch name must reflect intent, not execution order
- No unrelated changes
- Always branch from `main`
- Delete branch after merge

---

# 2.3 Branch Prefix Selection Rules (Mandatory)

Branch prefix must be selected according to the **intent of the change**, not the type of files modified.

---

## feat/ — New Feature

Use when implementing **new application functionality**.

Examples:

```
feat/nextjs-init
feat/blog-layout
feat/mdx-content-loader
feat/redis-metrics-api
feat/infinite-scroll
feat/dark-mode-toggle
```

Typical cases:

- New pages
- New APIs
- New UI features
- New logic
- External integrations

---

## fix/ — Bug Fix

Use when correcting behavior that **does not meet intended functionality**.

Examples:

```
fix/ip-dedupe-logic
fix/redis-ttl-calculation
fix/mdx-loader-parsing
```

Typical cases:

- Incorrect logic
- Runtime errors
- Broken UI
- Incorrect API results

---

## refactor/ — Internal Code Improvement

Use when **code structure changes but behavior remains identical**.

Examples:

```
refactor/content-loader
refactor/redis-client-wrapper
refactor/post-card-component
```

Typical cases:

- Extract modules
- Rename internal functions
- Remove duplication
- Simplify code

---

## chore/ — Maintenance

Use when changes affect **tooling or infrastructure only**.

Examples:

```
chore/eslint-config
chore/prettier-config
chore/typescript-config
chore/vercel-config
```

Typical cases:

- Lint rules
- formatting
- dependencies
- build config

---

## Prefix Priority

When uncertain:

```
feat > fix > refactor > chore
```

---

## Branch Slug Rules

Use **kebab-case**.

Good:

```
feat/blog-layout
feat/mdx-content-loader
fix/ip-dedupe-logic
```

Bad:

```
feat/blogLayout
feat/blog_layout
feat/BlogLayout
```

---

# 3. Commit Rules

- Commit per logical implementation unit inside the step.
- Keep the project buildable and test-passing after each commit.

---

# 4. Code Validation Requirements

## Mandatory Before PR

Before creating a PR:

- Build must pass
- Lint must pass
- Tests must pass
- Remove unused code
- Confirm alignment with PLAN definitions

If metrics/API logic exists:

- Validate against `Test Plan (Integration/API)`.

---

# 5. Pull Request Rules

## PR Title

Format:

```
[type] short description
```

Examples:

```
[feat] initialize Next.js project
[feat] implement blog layout
[feat] integrate Redis metrics
[fix] correct IP dedupe logic
```

---

## PR Template

```
## PLAN Reference
- PLAN.md Step: <Step Number>
- Related Sections:

## What
Implementation summary

## Why
Reason tied to PLAN

## Changes
Key files/modules

## Verification
Commands executed
Test results

## Screenshots (if UI)

## Acceptance Criteria Coverage

## Risk / Follow-up
```

---

# 6. Next Step Condition

Next step begins **only after PR merge**.

After merge:

1. Pull latest `main`
2. Create next branch
3. Continue sequentially

---

# 7. PLAN Status Management

## Step Format

```
- [ ] Step 1: Next.js project initialization
- [ ] Step 2: MDX loader
- [ ] Step 3: Blog routing
- [ ] Step 4: UI + Dark Mode
- [ ] Step 5: Redis metrics
- [ ] Step 6: Infinite scroll
- [ ] Step 7: Tests + deployment
```

## Completion Rule

Mark complete ONLY after:

- PR approved
- PR merged
- Acceptance criteria satisfied

Change:

```
- [ ] Step N
```

to

```
- [x] Step N
```

---

# Execution Guarantee

This workflow guarantees:

- Strict sequential execution
- PLAN.md as the authoritative specification
- No scope creep
- Deterministic branching
- Step-level traceability
- Acceptance criteria validation
