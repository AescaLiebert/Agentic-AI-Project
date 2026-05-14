# {Project Name} — Project Stack & Context (For AI Assistants)

> **Copy-paste this into any AI chatbot's system prompt or first message to bootstrap context.**

## Project Identity
- **Game:** "{Project Name}" — {genre description}
- **Engine:** {engine and version}
- **Platform:** {primary}-first, {secondary} port secondary
- **Core Fantasy:** {one-sentence core fantasy}

## Tech Stack
- {Input system}
- {Camera system}
- {UI framework}
- {Rendering/lighting approach}
- {Monetization if any}
- {Language / architecture constraints}

## Core Systems (Current State)
| System | Location | State |
|--------|----------|-------|
| {System Name} | `Assets/{path}` | {Prototype / Works / Needs refactor} |
| {System Name} | `Assets/{path}` | {state} |
| {System Name} | `Assets/{path}` | {state} |

## Architecture Rules (From GDD)
1. {Rule 1 — e.g., "No jump, no vertical platforming"}
2. {Rule 2 — e.g., "Mobile-first input"}
3. {Rule 3 — e.g., "Game feel > clean code"}
4. {Rule 4 — e.g., "Don't grow the god-object"}
5. {Rule 5 — e.g., "Data-driven content"}
6. {Rule 6 — e.g., "English-only identifiers, PascalCase"}

## Key Docs
- GDD: `Docs/GDD/GDD.md`
- ADRs: `Docs/ADRs/` — check before making architecture decisions
- DevLog: `Docs/DevLog/` — check recent entries for current state
- Rules: `Docs/RULES_AND_POLICY.md` — file org, naming, optimization, agent behavior

## Commit Convention
```
type(scope): description

Types: feat, fix, refactor, docs, style, test, chore, juice
Scopes: {your project scopes}
```
