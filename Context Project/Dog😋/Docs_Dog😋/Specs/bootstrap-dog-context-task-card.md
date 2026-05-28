---
slug: bootstrap-dog-context
status: needs-human
source: manual
gdd_tags:
  - identity
  - tech-stack
  - prototype-truth
  - architecture
  - guardrails
owner: codex
human_checkpoint: required
next_agent: human
blocked_by:
  - confirm-local-unity-project-files
  - confirm-networking-status
  - confirm-encoded-charm-item-name
  - confirm-test-scene-status
  - confirm-commit-scopes
---

# Task Card: Bootstrap Dog Project Context

## Player-Facing Goal

Future contributors should understand Dog's intended horror identity, mobile-first loop, current prototype reality, and target architecture before implementing or refactoring gameplay.

## Source

- Origin: manual prompt
- Link: `Context Project/Dog😋/GDD_Dog😋 Instruction.md`
- Requested by: project owner

## GDD Reference

- `@tag:identity` - Dog is a mobile-first 2D side-scrolling horror / puzzle / mystery descent game.
- `@tag:prototype-truth` - Existing Unity implementation is prototype-grade and singleton-heavy.
- `@tag:guardrails` - Preserve 2D side view, no jump, mobile-first input, Safe Room cadence, risky Room Search, and STALKER pressure.

## Type

- [ ] Feature
- [ ] Bug fix
- [ ] Refactor
- [ ] Code review
- [ ] Report / PM update
- [ ] Tooling / CI
- [x] Documentation / bootstrap

## Scope

### Systems Affected

- GDD
- AI context bootstrap
- Agent workflow routing

### Files To Inspect First

- `Context Project/Dog😋/GDD_Dog😋 Instruction.md`
- `Docs(Template)/GDD/GDD.md`
- `Docs(Template)/GDD/AI-Context/project-stack.md`

### Out of Scope

- Unity implementation changes.
- Build settings, dependency, CI, or package edits.
- Creative changes beyond the provided Dog source instruction.

## Acceptance Criteria

- [x] `Docs(Template)/GDD/GDD.md` is filled from the Dog source instruction.
- [x] `Docs(Template)/GDD/AI-Context/project-stack.md` is derived as a compact context index.
- [x] Unverified facts are flagged for human confirmation instead of guessed.
- [x] Follows `RULES_AND_POLICY.md` intent for context loading and source-of-truth safety.

## Human Checkpoints

- [x] Design/game-feel approval
- [x] Architecture approval
- [ ] PR review before merge
- [ ] Team status publish approval

## Router Decision

Recommended workflow: `/bootstrap-project`

Next artifact:

- Human review of `Docs(Template)/GDD/GDD.md` and `Docs(Template)/GDD/AI-Context/project-stack.md`
