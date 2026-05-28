# Multi-Agent Workflow Test Review

Date: 2026-05-18
Scope: `Docs(Template)/` against `multi_agent_workflow_design.md`
Audience: Solo/lead Agentic-AI game developer working with a team through Discord and Notion

> Implementation note: The P0/P1 recommendations from this review have been applied to the template set after this report was written. Treat the findings below as the audit trail that motivated the changes, not as the latest live-state checklist.

## Executive Verdict

The current template set can support Level 1 and Level 2 automation today: structured prompting, specialist agent roles, design/architecture/test artifacts, DevLogs, PR templates, and sprint reports. It is close to a Level 2.5 "manual orchestration plus repeatable workflows" setup.

It does not yet achieve Level 3 or Level 4 automation safely. The agent roles exist, but the system still needs a router/orchestrator, formal handoff contracts, team sync rules for Discord/Notion, rejection/rework loops, and actual Unity build/test automation before agents can reliably run end-to-end without you coordinating every handoff.

Recommended operating model:

- Use the repo markdown docs as the source of truth.
- Use Notion as a readable/team-facing mirror or planning board until a sync policy exists.
- Use Discord for notifications and human decisions, not as the canonical memory store.
- Keep human approval for design feel, architecture decisions, PR merge, CI/deployment changes, and any agent action involving secrets or destructive operations.

## Tested Workflow Simulation

### Test 0: Bootstrap Project

Simulated path:

1. Human fills `GDD/AI-Context/project-context-brief.md`.
2. AI reads the brief and `GDD/_TEMPLATE.md`.
3. AI writes `GDD/GDD.md`.
4. AI derives `GDD/AI-Context/project-stack.md`.
5. Human reviews the generated GDD and project stack.

Result: PASS with required human gate.

What works:

- `bootstrap-project.md` clearly explains why the templates must be filled before agent work.
- It directly supports token optimization by turning the GDD into an indexed context source.
- It protects against the "beautiful empty shell" problem by forcing real file paths, systems, and design rules.

Boundary:

- AI can expand and organize facts.
- AI must not invent core game content, inspirations, current code reality, or team decisions.

### Test 1: Implement Feature

Scenario: `Add parry timing system`

Simulated path:

1. Read relevant `GDD/GDD.md` tag.
2. Check ADR index.
3. Game Design Agent writes `Docs/Specs/parry-timing-design-spec.md`.
4. Human approves design/game-feel target.
5. Architect Agent writes `Docs/Specs/parry-timing-arch-plan.md`.
6. Human approves architecture and ADR if needed.
7. Implementation Agent writes code on a feature branch.
8. Code Review Agent reviews the diff.
9. QA Agent writes `Docs/TestPlans/parry-timing-test-plan.md`.
10. Human playtests feel and verifies the test plan.
11. DevLog and PR are created.

Result: PASS for Level 2 manual orchestration.

Blocked for Level 3:

- No orchestrator decides when to pause for design or architecture approval.
- No formal handoff schema proves the design spec contains every field the architect needs.
- No rework loop defines what happens when the human rejects the design, architecture, or playtest feel.

### Test 2: Fix Bug

Scenario: `Flashlight drains while paused`

Simulated path:

1. Read bug report.
2. Reference intended behavior from GDD.
3. Read project stack and relevant ADRs.
4. Reproduce.
5. Identify root cause.
6. Apply minimal fix.
7. Run regression and edge-case checks.
8. Commit and update DevLog.

Result: PASS for local/manual bugfix flow.

Gaps:

- Bug intake has no canonical source: GitHub issue, Notion task, Discord message, or manual prompt.
- No standard output path exists for a bug investigation note or regression result.
- The workflow does not call QA Agent to produce or update a test plan.

### Test 3: Refactor

Scenario: `Extract stamina logic from PlayerController`

Simulated path:

1. Define one-sentence refactor goal.
2. Check ADRs.
3. Architect Agent writes impact analysis to `Docs/Specs/{refactor-name}-impact-analysis.md`.
4. ADR is drafted if architecture changes.
5. Implementation Agent refactors incrementally.
6. Code Review Agent checks behavior preservation.
7. Regression checklist is executed.
8. DevLog and PR are created.

Result: PASS.

Gap:

- `.agent/workflows/refactor.md` exists, but `.agent/workflows/README.md` does not list it in the available workflow table.

### Test 4: Code Review

Simulated path:

1. Load project context.
2. Identify diff and GDD tag.
3. Run review checklist from `AgentPrompts/code-review-agent.md`.
4. Write review.

Result: PASS.

Optimization issue:

- `code-review.md` still says to read all of `RULES_AND_POLICY.md`, while the token policy says to load only relevant sections. For reviews, prefer diff plus `RULES_AND_POLICY.md` Sections 2, 5, 6, and 7 as needed.

### Test 5: Sprint Report

Simulated path:

1. GitHub Action extracts conventional commits.
2. Option A saves raw report to `Docs/Reports/{date}-sprint-report.md`.
3. Option B calls `scripts/format_sprint_report.py`.

Result: PARTIAL PASS.

What works:

- The script file exists.
- With UTF-8 enabled locally, the script writes the report and exits cleanly.
- Option A can produce a report without AI.

Blocked or incomplete:

- Option B is a stub and returns raw log, not a formatted PM report.
- Local Windows consoles that cannot encode emoji can fail on script output unless UTF-8 is enabled or script messages are ASCII.
- The workflow is configured for generated reports only, not Discord or Notion publishing.

### Test 6: Discord and Notion Team Flow

Simulated path:

1. Feature idea starts in Discord or Notion.
2. It becomes a canonical task.
3. Agents produce specs, plans, tests, and report updates.
4. Team receives status and review checkpoints.

Result: BLOCKED for automation.

Missing components:

- No source-of-truth policy for Notion versus repo markdown.
- No Discord notification map.
- No owner/reviewer assignment rules.
- No webhook or API integration spec.
- No rule for converting Discord discussion into a stable task artifact.

## Findings

### Overlap and Duplicate Content

| ID | Finding | Impact | Recommendation |
| --- | --- | --- | --- |
| O1 | Pipeline order appears in `multi_agent_workflow_design.md`, `AgentPrompts/README.md`, `.agent/workflows/implement-feature.md`, and `README.md`. | Drift risk and repeated context. | Keep `README.md` as the quick index and workflows as executable truth; make `multi_agent_workflow_design.md` conceptual only. |
| O2 | Feature request issue template overlaps with `GDD/AI-Context/feature-prompt-template.md`. | Two intake formats for the same thing. | Create one canonical task card schema and let GitHub/Notion/Discord map into it. |
| O3 | Review checklist appears in both workflow and agent prompt. | Small token waste and drift risk. | Keep checklist in `code-review-agent.md`; workflow should only call it. |
| O4 | PM report prompt overlaps with `Reports/_TEMPLATE.md`. | Low risk, but duplicate structure. | Keep report structure in `_TEMPLATE.md`; prompt should reference it. |
| O5 | `optimization_plan.md` is now stale and conflicts with the current folder state. | High confusion risk for future agents. | Mark it as historical/superseded or replace it with this report. |

### Conflicts

| ID | Conflict | Why It Matters | Fix |
| --- | --- | --- | --- |
| C1 | `multi_agent_workflow_design.md` still references `Docs/Architecture/parry-timing.md`, but current conventions save architecture plans to `Docs/Specs/{feature-name}-arch-plan.md`. | Agents may write artifacts to the wrong place. | Update the conceptual design doc or add a note that current paths are defined by `Specs/README.md`. |
| C2 | Context policy says all markdown gets context only from `GDD/GDD.md`, but bootstrap starts from `project-context-brief.md`. | Bootstrap is a valid exception, but not stated as an exception in the policy. | Add "Bootstrap Exception" to `RULES_AND_POLICY.md`. |
| C3 | `code-review.md` says to read full `RULES_AND_POLICY.md`; token budget says to load only relevant sections. | Review calls can become heavier than necessary. | Change code review loading to targeted sections. |
| C4 | The docs are valid UTF-8, but heavy emoji/box characters render as mojibake in some Windows console contexts. | Agents and scripts may display corrupted text or fail on output encoding. | Prefer ASCII in scripts and automation-critical docs; keep emoji only in human-facing docs if desired. |

### Low-Value or Useless Pieces

| ID | Item | Reason | Recommendation |
| --- | --- | --- | --- |
| U1 | `optimization_plan.md` as an active planning file. | It reports missing folders/scripts that now exist. | Archive or supersede it. |
| U2 | `CHANGELOG.md` recommended tools list. | Lists multiple tools but configures none. | Pick one tool later, or leave changelog manual until release automation matters. |
| U3 | Raw git-log examples repeated across report docs and workflow design. | Useful once, noisy after CI exists. | Keep command in `Reports/README.md`; remove extra copies during cleanup. |

### Blocked or Missing Components

| ID | Component | Status | Why It Blocks Higher Automation |
| --- | --- | --- | --- |
| B1 | Orchestrator/router prompt or script | Missing | Nothing decides which workflow and agent should run next. |
| B2 | Inter-agent handoff contracts | Missing | Agents produce markdown, but there is no required schema for downstream validation. |
| B3 | Rejection/rework loop | Missing | Level 3 automation needs defined paths for rejected design, architecture, QA, or game feel. |
| B4 | Discord/Notion integration policy | Missing | Team communication cannot be automated safely without source-of-truth rules. |
| B5 | AI-formatted sprint report implementation | Stub only | PM automation cannot produce stakeholder-ready reports yet. |
| B6 | Unity build/test automation | Not present in template | Level 4 requires CI to prove code still builds and tests pass. |
| B7 | Filled project GDD and project stack | Expected blank template | Agents cannot do useful work until bootstrap is completed for the real project. |

## AI Boundary Design

### AI May Do Automatically

- Draft design specs from approved GDD tags.
- Draft architecture plans from approved specs.
- Propose ADR drafts.
- Implement code only from approved design and architecture artifacts.
- Generate QA test plans.
- Generate raw or formatted sprint reports from git logs and DevLogs.
- Post status summaries after human-approved integration is configured.

### AI Must Ask Human Approval Before

- Changing the GDD source of truth.
- Choosing or changing core game pillars, player fantasy, tone, or game feel targets.
- Accepting architecture decisions or ADRs.
- Creating/altering CI, deployment, build settings, package dependencies, or secrets.
- Merging to `main`.
- Publishing Discord/Notion updates that represent official project status.
- Performing destructive file, git, or Unity operations.

### AI Must Not Do

- Invent current codebase facts not present in `project-stack.md`, GDD tags, or inspected files.
- Treat Discord chat as canonical project memory without converting it into a tracked task or doc.
- Fully automate game feel approval.
- Load the whole repo or all prompts by default.
- Mix feature work, bugfixes, and refactors in one PR unless a human explicitly approves the combined scope.

## Token Optimization Design

### Current Best Rule

Use `project-stack.md` as an index, not as a full memory dump. Then load only the active workflow, active agent prompt, relevant GDD tag, relevant ADRs, and current artifact.

### Recommended Context Packs

| Agent | Load | Avoid |
| --- | --- | --- |
| Router | Task card, project stack, workflow index | Full GDD, all prompts |
| Game Design | Project stack, one GDD tag, design prompt | Source code unless needed for constraints |
| Architect | Design spec, project stack, relevant ADRs, affected file list | Entire repo |
| Implementer | Approved design spec, approved arch plan, affected source files | All docs and all ADRs |
| Reviewer | Diff, relevant spec/ADR, review prompt, selected rules | Full files unless diff lacks context |
| QA | Design spec, implementation summary, affected systems | Full implementation if summary is enough |
| PM | Git log, DevLogs for period, report template | Full history or all reports |

### Token-Saving Changes

- Add a `Docs/Specs/{slug}-task-card.md` artifact as the single intake object for GitHub/Discord/Notion/manual prompts.
- Add frontmatter to specs: `slug`, `status`, `source`, `gdd_tags`, `owner`, `human_checkpoint`, `next_agent`, `blocked_by`.
- Mark old/obsolete docs as `status: superseded` so agents do not treat them as current.
- Replace duplicate checklist text with file references.
- Prefer ASCII for scripts and CI-generated output to avoid console encoding failures.

## Proposed Tested Workflow

This is the safest repeatable workflow for your current level:

1. Intake: Convert Discord, Notion, or GitHub discussion into one task card at `Docs/Specs/{slug}-task-card.md`.
2. Bootstrap Gate: Confirm `GDD/GDD.md` and `project-stack.md` are filled and free of placeholders relevant to the task.
3. Router: Choose one workflow: implement feature, fix bug, refactor, code review, or report.
4. Design: Game Design Agent writes `Docs/Specs/{slug}-design-spec.md`.
5. Human Design Checkpoint: You approve, reject, or request changes.
6. Architecture: Architect Agent writes `Docs/Specs/{slug}-arch-plan.md` and drafts ADR if needed.
7. Human Architecture Checkpoint: You approve, reject, or request changes.
8. Implementation: Implementation Agent changes only files listed in the approved architecture plan.
9. Review: Code Review Agent reviews the diff against spec, ADR, GDD, and relevant rule sections.
10. QA: QA Agent writes `Docs/TestPlans/{slug}-test-plan.md`.
11. Human Playtest: You verify game feel and subjective quality.
12. Team Review: PR goes to team; Notion/Discord receive status only after the PR and report are ready.
13. Report: PM Agent creates `Docs/Reports/{date}-sprint-report.md`; optional Discord/Notion sync runs after human-approved integration exists.

### Rework Loop

If rejected at any checkpoint:

- Design rejected: update task card or GDD tag, then rerun Game Design Agent.
- Architecture rejected: update design constraints or ADR decision, then rerun Architect Agent.
- Implementation rejected: keep specs fixed, rerun Implementation Agent only on affected files.
- QA rejected: file bug report, switch to `/fix-bug`, then rerun affected tests.
- Game feel rejected: update design spec with playtest notes, then iterate design and implementation only for tuning.

## Automation Readiness

| Level | Current State | Verdict |
| --- | --- | --- |
| Level 1: Structured prompting | Templates, GDD, ADRs, rules, commits exist. | Ready after bootstrap. |
| Level 2: Specialized roles | Six agent prompts and role workflows exist. | Ready with manual orchestration. |
| Level 3: Automation glue | Output paths exist, but router/contracts/rework/team sync are missing. | Not ready yet. |
| Level 4: CI/CD agent pipeline | Sprint report CI exists, but Unity CI and agent-triggered PR automation are missing. | Not ready yet. |

## Priority Fix List

### P0: Make Automation Safe

- Add an orchestrator/router document.
- Add handoff contract schema for task card, design spec, architecture plan, review, QA, and PM report.
- Add rejection/rework paths to workflows.
- Add Discord/Notion source-of-truth policy.
- Mark `optimization_plan.md` as superseded or historical.

### P1: Reduce Token Waste and Drift

- Update stale paths in `multi_agent_workflow_design.md`.
- Update `.agent/workflows/README.md` to list bootstrap and refactor.
- Make `code-review.md` load targeted rule sections instead of all rules.
- Consolidate feature intake around a single task card.

### P2: Improve Local and CI Automation

- Make `format_sprint_report.py` output ASCII-safe status messages or force UTF-8.
- Implement the AI formatting call or explicitly label Option B as inactive.
- Add Unity build/test workflow when the actual Unity project is available.
- Add optional Notion/Discord publish scripts only after source-of-truth policy is chosen.

## Final Assessment

This is a strong foundation for an Agentic-AI game development workflow. It already reduces chaos by separating design, architecture, implementation, review, QA, and reporting. The biggest remaining risk is not the agent prompts; it is governance between agents and humans.

For your solo-designer plus team setup, the winning version is not "agents do everything." It is "agents prepare the work, humans make taste and risk decisions, and automation moves the boring artifacts between places." With the P0 fixes, this can become a reliable Level 3 workflow. Without them, it remains a good manual system that still depends on you remembering every handoff.
