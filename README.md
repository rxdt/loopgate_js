<div align="center">

<img src=".banner.svg" alt="Blue infinity loop" width="360">

<h1>LoopGate JS</h1>
<p><em>TypeScript frontend coding-agent loop harness</em></p>
<p>LoopGate JS is a repo-native TypeScript and Vite harness for running autonomous coding-agent loops with Claude, Codex, Copilot, Agy, and other CLI agents. Agents can edit. Gates decide what lands. You set the task, the loop feeds the prompt, and each iteration must commit through lint, tests, Playwright, Lighthouse, coverage, security, and containment guardrails.</p>

![Claude](https://img.shields.io/badge/Claude-D97757?style=for-the-badge&logo=claude&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white)
![Node](https://img.shields.io/badge/Node-22+-339933?logo=nodedotjs&logoColor=white)
![pnpm](https://img.shields.io/badge/pnpm-10+-F69220?logo=pnpm&logoColor=white)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](https://makeapullrequest.com)
[![](https://img.shields.io/badge/code%20style-mine-999)](https://github.com/sebmestrallet/absurd-badges)
[![](https://img.shields.io/badge/created%20an%20AGI%20by%20mistake-no-3C1)](https://github.com/sebmestrallet/absurd-badges)
![X (formerly Twitter) Follow](https://img.shields.io/twitter/follow/roxdtvc)

---

## [Live deploy of sample app here](https://aideploymentcalculator.vercel.app/)

### [Hands off, agent-only sample app code here](https://github.com/rxdt/ai_deployment_calculator)

</div>

---

## What is LoopGate JS?

LoopGate JS, also published as `loopgate_js`, is a frontend loop harness for
AI coding agents. It turns a normal Vite + TypeScript repository into a
repeatable agent workflow: prompts live in the repo, specs define the work,
agents run in fresh iterations, and every result has to pass the same quality
gate before it can land.

Use it when you want Claude Code, OpenAI Codex, GitHub Copilot, Agy, or another
CLI coding agent to build frontend code inside a strict JavaScript/TypeScript
quality loop instead of making one-off edits.

## TL;DR: Getting Started

1. `git init` (the gate, hooks, and loop all shell out to git)
2. `pnpm setup` installs dependencies, merges harness scripts into the root `package.json`, sets `core.hooksPath` to `.githooks`
3. Write your project goal in [docs/plan.md](docs/plan.md)
4. `pnpm loop <claude|codex|agy|copilot> [max_iterations] [max_minutes]`
5. Not what you wanted? Refine [docs/plan.md](docs/plan.md) / [PROMPT.md](PROMPT.md) and re-run

---

## Features

- Worker-agnostic AI coding-agent loop: Claude, Codex, Copilot, Agy, or any prompt-reading CLI
- Built-in quality gates: agents work only if they pass the quality bar you set ✅
- Repo-as-memory workflow: specs/status/prompt are durable but code is king, leaving you free 😎
- Frontend-ready TypeScript seed app: Vite, strict TypeScript, inline production build, and a replaceable `frontend/src/main.ts` entry point
- Browser quality bar: HTML validation, ESLint, Stylelint, dependency-cruiser, Knip, Semgrep, Secretlint, and `pnpm audit` **AND SO MUCH MORE**
- Test harness included: Vitest with hard 100% coverage, Playwright across desktop and mobile viewports, Lighthouse SEO budgets, jsdom, fast-check, and axe-core
- Preflight vs full gate split 🆗
- Forbidden-path containment: Don't touch that!-configurable 🛑
- Installable project template: harness install loopgate gets the repo ready ▶️
- Fresh-context agent iterations to reduce context rot 🔄
- One command setup gets you git hooks and everything else
- Timeouts and time-limits for all loops ⏸
- Agent containment prioritized: Stop the madness (and Semgrep for safety) 🔓

## Why frontend loops are harder

Backend loops can be judged with deterministic inputs and outputs. Frontend work adds a messier contract. The app must build, render, respond, fit, remain accessible, and keep doing that across viewports and loading paths. Not to mention it's gotta have ✨ taste ✨

That's why this harness is heavier than a plain TypeScript setup. A frontend agent can pass unit tests while shipping a blank page, broken layout, focus state, shoddy HTML, missing prod assets, or brittle styles. The gate has to check things more like a human would, not just as a code parser. So the loop is strict on purpose:

- TS, HTML, CSS, JSON format
- lint
- types
- architecture
- dead code
- security
- build
- unit coverage
- e2e Playwright
- Lighthouse must be 100
- preferences.ts checking for smells that an app is not responsive...
- etc.

They all cover different failure modes. None of them solo is enough when coding with agents.

_**[See the Python version of this loop harness here](https://github.com/rxdt/loopgate_harness)**_

---

## Details

`PROMPT.md` tells each agent to pick a `spec` and build. `docs/specs/` say _what_ to build. The agent decides _what next_. You keep `docs/plan.md` current, and specs get rewritten from it (agent is told in `PROMPT.md` to update the specs). Each iteration the agent updates its spec and `docs/PROJECT_STATUS.md`. Ideas from [ghuntley](https://github.com/ghuntley), How to Ralph Wiggum.

> [!TIP]
> If you don't like _ANYTHING_ in this framework, remove it.

## Start a new project

1. Copy this directory (or use it as a GitHub template), then `git init` and `node harness/harness.mjs setup`.
2. Make it yours: set `name` in `package.json` (root), and the `<title>` in `frontend/index.html`.
3. Write your grand vision into `docs/plan.md`.
4. Add your first spec in `docs/specs/` (see [Creating Your First Spec](docs/creating-specs.md)), or have an agent draft the first specs.
5. Replace the seed app. `frontend/src/main.ts` and its unit test `frontend/tests/home.spec.ts` are a minimal wired-up "Hello, world" proving the whole gate. Grow them, don't delete them.
6. Strict TypeScript, ESLint, Stylelint, Vitest @ 100% coverage, Playwright, and Lighthouse budgets are configured in `harness/`.
7. Your coding preferences go in `harness/preferences.ts`.
8. Run a loop:

```sh
pnpm loop <agent> [max_iterations] [max_minutes]  # agent: claude/codex/agy/copilot
```

## A l∞ps Loop

The repo is the only memory. Each iteration is a fresh-context agent.

- `docs/specs/` say WHAT to build
- constant `PROMPT.md` tells the agent: read `docs/specs/`, review the code, build the most important unfinished thing
- agent builds
- agent commits
- every git commit passes the fast preflight (format, lint, plus loop containment for the agent)
- every git push runs the full gate: lint, types, build, security scans, tests, 100% coverage, e2e, Lighthouse
- the loop stops at `max_iterations`, a nonzero worker exit, or a timeout
- Unspecified iterations/minutes default to 2 iterations × 20 minutes each
- **The harness is worker-agnostic.** Any agent CLI that reads a prompt from stdin and can edit/commit works.
- Each run is logged to `scratchpad/runs/<agent>/<date>/NNNN.jsonl`

## Safety

[`harness/ralph.sh`](harness/ralph.sh) launches an autonomous LLM worker with the permissions granted in the `AGENTS` presets at [harness/cli.ts:24](harness/cli.ts#L24), e.g. `--dangerously-skip-permissions` or `--sandbox danger-full-access`.

The gate bounds what any **commit** may touch, but the worker itself is **not** truly sandboxed to this repo. Consider the balance: without access it cannot do much. With machine access it can wreak havoc. Under a permissive mode it can run arbitrary shell. You are authorizing real changes. Choose the worker and permission mode deliberately.

### The Gate: Tiered Checks

[`harness/gate-data.ts`](harness/gate-data.ts) holds `FORBIDDEN_DIRS`, `FORBIDDEN_FILES`, `FORBIDDEN_BASENAMES`, and `FORBIDDEN_PATTERNS`. [`harness/preferences.ts`](harness/preferences.ts) holds the human's style checks other tools can't catch. Containment runs when `RALPH_LOOP=1`, which `ralph.sh` sets on each run. Humans own the harness (`harness/` is itself a forbidden path).

⚡ `pnpm preflight` (pre-commit) runs the four checks marked ⚡ below. When `RALPH_LOOP=1` it additionally runs, on staged files only: **containment** (un-stages forbidden paths/symlinks, fails on forbidden patterns — see below) and the **preference** AST checks (next section).

✅ `pnpm gate` (CI / pre-push) runs every check in [`FULL_CHECKS`](harness/gate-data.ts):

| Check          | Tool                                                                                                                                                                                                  | ⚡  |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| `format`       | `prettier --check` over `frontend/` + `harness/`                                                                                                                                                      | ⚡  |
| `eslint`       | `eslint --max-warnings=0` (typescript-eslint + unicorn, sonarjs, security, regexp, promise, import-x, jsdoc, n, no-only-tests)                                                                        | ⚡  |
| `style`        | `stylelint` over `frontend/**/*.css` (standard, strict-value, defensive-css, custom-media)                                                                                                            | ⚡  |
| `html`         | `html-validate` over all HTML                                                                                                                                                                         | ⚡  |
| `typecheck`    | `tsc` strict over app source (`harness/tsconfig.app.json`)                                                                                                                                            |     |
| `harnessTypes` | `tsc` strict over the harness itself (`harness/tsconfig.harness.json`)                                                                                                                                |     |
| `schema`       | `ajv` compiles `frontend/schemas/**/*.schema.json` (passes when the directory is absent)                                                                                                              |     |
| `cruise`       | `dependency-cruiser` architecture rules over `frontend/src`                                                                                                                                           |     |
| `deadcode`     | `knip` — unused files, exports, types, dependencies                                                                                                                                                   |     |
| `spelling`     | `cspell` (advisory: warns, never fails)                                                                                                                                                               |     |
| `workflow`     | `spectral` lints `.github/workflows/ci.yml`                                                                                                                                                           |     |
| `sast`         | `semgrep` p/typescript + p/javascript + p/security-audit (external tool: `brew install semgrep` — skipped when absent; the npm "semgrep" package is an empty name-reservation stub, don't install it) |     |
| `secrets`      | `secretlint`                                                                                                                                                                                          |     |
| `audit`        | `pnpm audit --audit-level high`                                                                                                                                                                       |     |
| `build`        | `vite build` (CSS/JS inlined into `index.html`)                                                                                                                                                       |     |
| `coverage`     | `vitest` with **hard 100%** line/branch/function coverage                                                                                                                                             |     |
| `e2e`          | `playwright` — 6 viewports, desktop + mobile, specs in `frontend/tests`                                                                                                                               |     |
| `lighthouse`   | `lhci` (Lighthouse) performance/a11y/best-practices/SEO against the built page                                                                                                                        |     |

Deliberately absent, with reasoning in [gate-data.ts](harness/gate-data.ts) comments: a bundle **size budget** (removed with the old size subsystem; noted as a FOLLOW-UP to re-add), **osv-scanner** (disabled — template deps track `latest`, transitive-CVE noise), **lockfile-lint** (no pnpm parser; pnpm itself blocks the injection vector it guarded), and **syncpack** version consistency (deps intentionally track `latest`).

Shipped for your tests, not separate gate checks: **`fast-check`** (property-based testing) and **`@axe-core/playwright`** (accessibility assertions for e2e specs) are devDependencies in both packages; **`jsdom`** provides the DOM for unit tests (opt-in per file with `// @vitest-environment jsdom`, as the seed test does). On pull requests, GitHub CI additionally runs **`dependency-review-action`** (fails on moderate+ vulnerabilities and license problems).

Only humans can bypass triggered gates and commit by adding flag `--no-verify`.

### Preferences: what the tools above can't check

[`harness/preferences.ts`](harness/preferences.ts) is where the human encodes rules none of the tools express. During loop preflight (`RALPH_LOOP=1`) it walks the AST of every staged TypeScript file and fails the commit on:

- **DOM selector discipline** — `querySelector` / `querySelectorAll` / `closest` / `matches` must receive a string literal containing exactly one `data-*` attribute selector from the allowlist at the top of the file. Dynamic selectors, class/id selectors, compound selectors, and unlisted `data-*` hooks all fail. (No ESLint rule validates selector strings against a project allowlist.)
- **Layout-read ban** — no measuring the page from TypeScript: `getBoundingClientRect()`, element reads (`offsetWidth/Height/Top/Left`, `clientWidth/Height`, `scrollWidth/Height`), and viewport reads (`window.innerWidth/innerHeight`) are rejected. Layout belongs in CSS.

Add your own checks there; the file is containment-protected, so agents can't relax it.

### Containment

When `RALPH_LOOP=1`, preflight un-stages any commit touching `harness/`, `.githooks/`, `.github/`, any `package.json`, lockfiles, tool configs, `AGENTS.md`, `docs/plan.md`, or `PROMPT.md` — and rejects escape hatches in staged content (`eslint-disable`, `ts-ignore`, `test.skip`, `.only(`, coverage-ignore pragmas, `--no-verify`, `hooksPath`, …). Full lists: [harness/gate-data.ts](harness/gate-data.ts).

## Layout

```
harness/        the gate, loop (ralph.sh), CLI, tool configs, custom user checks  (🤖 forbidden)
    gate.ts                  the gate engine: containment + check runner
    gate-data.ts             containment denylists + the check registry
    preferences.ts           user-defined AST checks not covered by tools
    *.test.ts                the harness's own tests
    eslint.config.js         eslint       — TS/JS lint bar
    stylelint.config.js      style        — CSS bar (scope: .stylelintignore)
    .htmlvalidate.json       html         — HTML bar
    .prettierignore          format       — prettier scope (rules: prettier defaults)
    tsconfig.app.json        typecheck    — strict flags for frontend source
    tsconfig.harness.json    harnessTypes — strict flags for the harness itself
    vite.config.ts           build + dev  — Vite root, inline-assets plugin
    vitest.config.js         coverage     — test discovery, hard 100% thresholds
    playwright.config.js     e2e          — 6 browser profiles, dev-server port
    lighthouserc.cjs         lighthouse   — perf/a11y/SEO budgets, preview port
    .dependency-cruiser.cjs  cruise       — architecture rules
    knip.json                deadcode     — unused files/exports/types
    cspell.json              spelling     — word allowlist (advisory)
    .spectral.yml            workflow     — CI-workflow lint rules
    .secretlintrc.json       secrets      — secret-scanning preset
.githooks/      pre-commit (preflight) / pre-push (gate) hooks                    (🤖 forbidden)
.github/        CI that re-runs the gate + PR dependency review                   (🤖 forbidden)
tsconfig.cruise.json  root shim for cruise (extends harness/tsconfig.app.json)    (🤖 forbidden)
pnpm-workspace.yaml   workspace members, allowed build scripts, version floors    (🤖 forbidden)
.htmlvalidateignore   scope of the html check (read from the scan root)           (🤖 forbidden)
AGENTS.md       rules for agents working in the repo                              (🤖 forbidden)
PROMPT.md       the standing per-iteration instruction                            (human maintained)
docs/plan.md    the human vision                                                  (human maintained)
docs/specs/     WHAT to build, one PRIORITY-bannered file per track
docs/PROJECT_STATUS.md  human-readable record, not authoritative
scratchpad/     scratch dir agents can use for temp files + run logs              (🤖 for the bots)
frontend/       your product code: index.html, public/, src/, tests/ (e2e specs)
```

If an agent edits a forbidden file, the file will be un-staged (not allowed to commit). A forbidden pattern by an agent (e.g. `ts-ignore`) will also prevent their commit and force them to fix it.

## Why every check has its own config file

Deliberate design, not clutter:

- **Every gate command pins its config explicitly** (`--config`, `--ignore-path`, `-p` — see [`FULL_CHECKS`](harness/gate-data.ts)). Nothing relies on auto-discovery, so the file the gate names is provably the file that governs the check.
- **They all live in `harness/`, one forbidden directory.** A single containment rule protects the entire quality bar — thresholds, lint rules, coverage floors — from the agents it judges.
- **Placement doubles as defense.** Tools' own config discovery walks _up_ from the linted file; `harness/` is never on that path. If an agent drops a look-alike `eslint.config.js` at the repo root, the explicit `--config` still wins.
- **`package.json` is forbidden by basename everywhere** because scripts and dependencies define what the gate actually executes.
- **Binaries are pinned too**: checks run with `harness/node_modules/.bin` prepended to `PATH` ([gate.ts](harness/gate.ts) `checkEnvironment`), so an agent can't shadow a tool with its own executable.
- **Three root-level exceptions exist because the tools force them** — each is individually in `FORBIDDEN_FILES`: `tsconfig.cruise.json` (dependency-cruiser resolves tsconfig includes from its repo-root cwd), `pnpm-workspace.yaml` (pnpm requires it at the workspace root), and `.htmlvalidateignore` (html-validate only discovers ignore files from the scan root).

## ⚠️ Warnings. Read this before a first run.

1. **This harness does not sandbox agents.** It _tries_ to harness bad code in loops via gates. Sandboxing agents will, e.g. prevent them from maintaining git, running Playwright, being seen as trustworthy by semgrep leading to cyclical failures, etc.

2. **The gate is a guardrail, not a jail.** Agents are crafty, like people. They will find a way to complete a task at all costs. **Trust nothing and no one.**

3. **Mind your usage limits.** `ralph.sh` works agents to the cap set. You can easily burn through your tokens, context windows, and provider usage limits. **Workers continue running as long as there is work to do.**

4. **`PROMPT.md` tells the worker to push every iteration.** Protect `main` and run the loop on its own branch.

5. **100% coverage does not mean good tests.** That is quantity, not quality.

6. **Some gate checks need content to exist.** The template ships a minimal seed app (`frontend/src/main.ts`, a unit test, an e2e spec) because coverage, e2e, and Lighthouse fail on an empty project. Replace the seed; don't leave `frontend/src/` or `frontend/tests/` empty.

## Commands

Check commands are defined in [harness/gate-data.ts](harness/gate-data.ts).

```sh
pnpm setup      # install deps, merge root scripts, set core.hooksPath to .githooks
pnpm preflight  # fast checks: format, lint (plus loop containment)
pnpm gate       # full pass: see the table above
pnpm status     # render run/gate status
pnpm loop <agent> [max_iterations] [max_minutes] [verbose]  # defaults: 2 20 true
RALPH_LOOP=1 pnpm gate  # run as if you are the agent in the loop

# After setup, root package.json has aliases: pnpm gate / pnpm lint / pnpm test / pnpm loop / pnpm status

# UNDERLYING AGENT CALLS (presets defined in AGENTS at harness/cli.ts:24)
harness/ralph.sh 10 20 claude -p --dangerously-skip-permissions --no-session-persistence --output-format stream-json --verbose

harness/ralph.sh 2 20 env -u CODEX_THREAD_ID -u CODEX_CONVERSATION_ID -u CODEX_SESSION_ID codex exec -m gpt-5.5 --json --sandbox danger-full-access -

harness/ralph.sh 3 10 agy --log-file agy.log -p --dangerously-skip-permissions

harness/ralph.sh 2 20 sh -c 'copilot --output-format json --stream on --allow-all-tools -p "$(cat)"'
```

## Expanding your harness

- Edit tool configs in `harness/`: [eslint.config.js](harness/eslint.config.js), [stylelint.config.js](harness/stylelint.config.js), [vitest.config.js](harness/vitest.config.js), [playwright.config.js](harness/playwright.config.js), [lighthouserc.cjs](harness/lighthouserc.cjs), [.dependency-cruiser.cjs](harness/.dependency-cruiser.cjs), [knip.json](harness/knip.json), [cspell.json](harness/cspell.json), [tsconfig.app.json](harness/tsconfig.app.json)
- Add forbidden files, directories, or patterns in [gate-data.ts](harness/gate-data.ts)
- Add or remove structural checks in [preferences.ts](harness/preferences.ts)
- Edit the check registry in [gate-data.ts](harness/gate-data.ts) and CI in [ci.yml](.github/workflows/ci.yml)
- `semgrep` uses registry configs (`p/typescript`, `p/javascript`, `p/security-audit`); it needs network and a machine-level install

## CI Troubleshooting

**Playwright browser mismatch in GitHub Actions**
If Playwright tests fail in CI because multiple browser projects are configured but only one browser is installed, run `pnpm --prefix harness run setup:e2e` in your CI workflow. This command installs the browser set expected by the Playwright config.

## Coordination

- Use `git log --oneline <branch>..HEAD` to show what's unpushed.
- There is NO worktree/branch creation by design. You can create branches/trees and run a loop in each, then merge _(if you really feel like managing that)_
- Agent duties can be contained to a part of the repo. e.g. give each agent its own `docs/specs/<track>.md` (one spec file per track)...

### If you must be a ringleader

**Recommendations for running several agents at once on one branch (no worktrees):**

- **You (human):** seed each spec once with this exact line near the top:

  ```
  Spec claimed by agent: <unclaimed>
  ```

- **The agents:** paste this exact block into [PROMPT.md line 3](PROMPT.md#L3):

  ```
  Other agents are working this repo. Before touching code, pick a spec whose claim line is
  <unclaimed>, replace it with your name, and commit that claim first. Own that spec's file and its
  tests. Set the line back to <unclaimed> on your last commit.
  ```

- What fails when agents do not claim specs/work: agents all pick the top-priority spec, duplicate work, and leave a half-staged git index.
- What fails with too little time i.e. MAX_MINUTES too low: a worker dies mid-`gate` before it can commit, and its spec claim stays STUCK until a human resets the line to `<unclaimed>`. Give each iteration enough minutes to finish (the gate itself takes a while). **Note:** The full gate alone takes a couple minutes
- Do not rely on agent names for coordination: agents self-name inconsistently and can collide. Names are for human blame/log-matching only; the claim line + committed code are what actually coordinate.

- Which doc does what:
  - **specs** = the product work
  - **`PROMPT.md`** = how to operate headlessly
  - **repo + green gate** = the source of truth
  - `docs/PROJECT_STATUS.md` is a human-readable record, not authoritative
