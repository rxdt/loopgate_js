# Creating Your First LoopGate Spec

This guide explains how to turn a project idea into a clear, actionable specification for autonomous coding agents in LoopGate JS.

---

## 1. How Documentation Fits Together

LoopGate JS relies on three core documents under `docs/` to organize work between humans and AI agents:

| Document | Purpose | Owner |
| :--- | :--- | :--- |
| **`docs/plan.md`** | **The Grand Vision**: High-level objective, features, technical approach, and major project milestones. | Human (or delegated to agents) |
| **`docs/specs/*.md`** | **Actionable Tasks**: Concrete, track-specific instructions defining WHAT to build, test criteria, and boundaries. | Human / Agent |
| **`docs/PROJECT_STATUS.md`** | **Current State**: Snapshot of active focus, pass/fail status of gates, next actions, and changelog. | Human & Agent (updated each iteration) |

---

## 2. Step-by-Step: From Idea to Spec

### Step 1: Align with `docs/plan.md`
Check `docs/plan.md` to determine which milestone or feature you are addressing. Ensure the objective is well-defined.

### Step 2: Copy the Base Template
Create a new file in `docs/specs/` using `docs/specs/base.md` as the template:

```bash
cp docs/specs/base.md docs/specs/dark-mode-toggle.md
```

### Step 3: Fill in the Sections
1. **Priority Header**: State priority (`PRIORITY 1`, `2`, or `3`) and a 1–2 sentence problem statement.
2. **Scope**: Detail frameworks, APIs, files, or CSS classes to use.
3. **Priorities / Milestones**: Break the work into concrete sub-tasks with verifiable definitions of done (e.g., test commands).
4. **Guardrails**: Define constraints (e.g. use CSS custom properties, avoid measuring DOM via TypeScript).
5. **Acceptance Criteria**: State measurable checks (e.g., `pnpm gate` passes, 100% test coverage).
6. **Out of Scope**: Explicitly list non-goals to prevent agent scope creep.

### Step 4: Update `docs/PROJECT_STATUS.md`
Point the active focus to your new spec (e.g., `Active spec: docs/specs/dark-mode-toggle.md → Milestone 1`).

---

## 3. Example Spec: Frontend Dark Mode Toggle

Here is a minimal, complete example of a frontend feature spec (`docs/specs/dark-mode-toggle.md`):

```markdown
# Dark Mode Toggle Spec

> **PRIORITY 1.** Add a theme toggle button in the header that switches between light and dark themes using CSS variables and persists preference in localStorage.

## Scope
- Modify \`frontend/index.html\` to include the theme toggle button in the header.
- Add theme color variables (\`--bg-primary\`, \`--text-primary\`) in \`frontend/src/style.css\`.
- Add event listeners and localStorage persistence in \`frontend/src/theme.ts\`.

## Priorities

1. Milestone 1: Theme State Management & CSS Tokens
   - Define CSS custom properties for \`[data-theme="dark"]\` and \`[data-theme="light"]\`.
   - Implement \`initTheme()\` and \`toggleTheme()\` helper functions.
   - Files created or updated: \`frontend/src/style.css\`, \`frontend/src/theme.ts\`
   - Definition of done: \`pnpm --filter frontend test -- src/theme.test.ts\`

2. Milestone 2: UI Integration & Accessibility
   - Mount button with \`aria-label="Toggle dark mode"\` and \`data-hook="theme-toggle"\`.
   - Update e2e tests in \`frontend/tests/home.spec.ts\` to verify theme toggling across viewports.
   - Definition of done: \`pnpm gate\` exits 0.

## Guardrails
- Use semantic CSS custom properties; do not hardcode hex values in component styles.
- Selector discipline: Target the button using \`data-hook="theme-toggle"\`.
- Keep bundle lean; do not introduce third-party icon packages.

## Acceptance Criteria
- Clicking the toggle flips \`data-theme\` attribute on \`<html>\` or \`<body>\`.
- Theme selection persists across page reloads via \`localStorage\`.
- Unit tests achieve 100% line and branch coverage.
- \`pnpm gate\` passes cleanly.

## Out of Scope
- System OS theme detection / matchMedia auto-switching (Milestone 2 follow-up).
- Animation transitions for theme change.

## Changelog
- Initial draft of dark mode toggle spec.
```

---

## 4. Verifying Specs

Before running autonomous loops, ensure the project satisfies the gate:

```bash
pnpm preflight   # Fast format and lint checks
pnpm gate        # Full validation suite (types, unit tests, e2e, security)
```
