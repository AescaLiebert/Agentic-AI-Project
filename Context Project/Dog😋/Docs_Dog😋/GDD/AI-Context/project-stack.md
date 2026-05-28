# Dog - Project Stack & Context

Load this first, then load only the relevant `@tag:` section from `Docs(Template)/GDD/GDD.md`.

## Identity
**Dog** is a mobile-first 2D side-scrolling horror / puzzle / mystery game. A cute anime girl descends from a 100th-floor condo through a collapsed, monstrous world. PC is a mapped port layer. Engine: Unity 6000.3.4f1, source-provided.

## Stack
URP 2D lighting, Unity Input System, Cinemachine, UGUI + TextMeshPro, `Light2D` flashlight play, Unity Purchasing, Unity Test Framework. Networking is undocumented; confirm whether none.

## Current Prototype
Core source paths from the Dog instruction: `Assets/_custom/Scrip/Player.cs`, `GameManager.cs`, `SceneInitializer.cs`, `LoedScene.cs`, `roomManage.cs`, `ItemPickUp.cs`, `Flaslight/*`, `Enemy/*`, and `Assets/_custom/Stor/*`. The prototype is playable but singleton-heavy, scene-string-driven, typo-prone, and not the target architecture.

## Architecture Rules
Preserve 2D side view, left/right movement, no jump, mobile-first input, risky Room Search, Safe Rooms every 5 floors, and STALKER pressure. Do not grow `GameManager`. Separate prototype truth from target architecture. Move toward data-driven items, enemies, floors, encounters, and shops. Use English identifiers, PascalCase classes, and one owner per responsibility.

## Key Tags
Use `@tag:identity`, `@tag:platform-input`, `@tag:core-loop`, `@tag:tech-stack`, `@tag:prototype-truth`, `@tag:architecture`, `@tag:guardrails`, and `@tag:roadmap`.

## Needs Human Confirmation
Local `Assets/`, `Packages/`, and `ProjectSettings/` are absent. Confirm script paths, packages, build scenes, networking status, `Test.unity`, encoded charm item name, and commit scopes.

## Commit Convention
Use `type(scope): description`; types: `feat`, `fix`, `refactor`, `docs`, `style`, `test`, `chore`, `juice`. Proposed scopes mirror `@tag:system-ownership` and need human confirmation.
