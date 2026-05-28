# {Project Name} - Project Documentation Hub

All documentation lives here, outside Unity Assets, version-controlled alongside the codebase.

## Folder Structure

```text
Docs/
  GDD/                         # Game Design Document + AI context files
    _TEMPLATE.md               # Copy to GDD.md and fill in your game's details
    AI-Context/
      project-context-brief.md # Fill this first
      project-stack.md         # Auto-derived from GDD
      feature-prompt-template.md
  ADRs/                        # Architecture Decision Records
    _TEMPLATE.md
    001-*.md
  Specs/                       # Task cards, design specs, architecture plans
    README.md
    {slug}-task-card.md
    {slug}-design-spec.md
    {slug}-arch-plan.md
  TestPlans/                   # QA test plans and regression reports
    README.md
  Handoff-Contracts/           # Inter-agent schemas and status gates
    README.md
    task-card-template.md
  DevLog/                      # Git-ready development logs
    _TEMPLATE.md
    YYYY-MM-DD-*.md
  Reports/                     # Auto-generated PM reports and reviews
    _TEMPLATE.md
  AgentPrompts/                # System prompts for each AI agent role
    orchestrator-agent.md
    game-design-agent.md
    architect-agent.md
    implementation-agent.md
    code-review-agent.md
    qa-agent.md
    pm-report-agent.md
  .agent/workflows/            # IDE workflow automation
    bootstrap-project.md
    implement-feature.md
    fix-bug.md
    code-review.md
    refactor.md
    report.md
  .github/                     # GitHub templates and CI/CD
    PULL_REQUEST_TEMPLATE.md
    COMMIT_CONVENTION.md
    ISSUE_TEMPLATE/
    workflows/
  scripts/                     # Automation scripts and CI helpers
    format_sprint_report.py
  CHANGELOG.md
  RULES_AND_POLICY.md          # AI rules, hierarchy, optimization, token policy
  TEAM_SYNC_POLICY.md          # Discord/Notion/source-of-truth rules
  multi_agent_workflow_design.md
  README.md
```

## How to Use

0. Bootstrap once per project: fill `GDD/AI-Context/project-context-brief.md`, then run `/bootstrap-project`.
1. Confirm `GDD/GDD.md` and `GDD/AI-Context/project-stack.md` are filled with real project context.
2. Start every task from `Docs/Specs/{slug}-task-card.md`, using `Handoff-Contracts/task-card-template.md`.
3. Route the task with `AgentPrompts/orchestrator-agent.md`.
4. Run one workflow: `/implement-feature`, `/fix-bug`, `/refactor`, `/code-review`, or `/report`.
5. Stop at human checkpoints for design feel, architecture, PR merge, team publishing, and risky automation.
6. After each session, write a DevLog entry from `DevLog/_TEMPLATE.md`.
7. Weekly, generate reports using `/report` or `.github/workflows/sprint-report.yml`.
8. Copy `RULES_AND_POLICY.md` into your AI tool config.

## Context Policy

All normal project work gets design context from `GDD/GDD.md` and its `@tag:` markers.

Bootstrap is the only exception: `project-context-brief.md` is allowed to create the initial `GDD/GDD.md` and `project-stack.md`.

For Discord and Notion usage, follow `TEAM_SYNC_POLICY.md`: repo markdown is canonical, Notion is a planning/readability layer, and Discord is discussion/notification until converted into a tracked repo artifact.

## Token Policy

Agents should load only:

- `GDD/AI-Context/project-stack.md`
- Current task card or current artifact
- Relevant GDD `@tag:` section
- Current agent prompt only
- Relevant ADRs or affected source files only when needed

Do not load all prompts, all docs, or the full repo by default.
