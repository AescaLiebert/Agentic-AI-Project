# Neo-JJK - Game Design Document

## Purpose

This document is the current design authority for **Neo-JJK**.

`GDD_JJK.md` is a legacy plan. Keep it as historical reference, but do not use it as the active combat direction unless a human explicitly asks for legacy comparison work.

Neo-JJK has shifted away from action/fighting-game hybrid combat and into a simpler **2D side-scrolling action hack-and-slash rogue-lite**. The design target is fast, readable, flashy, and easy to build.

The most important production constraint is:

- **Low development effort**
- **Casual low skill floor**
- **Casual low skill ceiling**
- **Simple logic over clever systems**
- **VFX impact over animation complexity**

If a feature requires complex combat state, strict timing rules, advanced combo routing, or high execution skill, it is outside the current design direction.

---

<!-- @tag:identity -->
## Project Identity

| Field | Direction |
| --- | --- |
| Working Title | **Neo-JJK** |
| Genre | **2D side-scrolling action hack-and-slash rogue-lite** |
| Core Fantasy | Clear chaotic side-scrolling combat rooms with a small team of flashy characters, swapping them to spend skills, apply buffs, and burst enemies down. |
| Player Promise | Fast basic action, readable attacks, big VFX, simple upgrades, and easy team rotations. |
| Skill Target | **Casual low floor and low ceiling**. The game should feel good quickly and should not demand fighting-game execution. |
| Development Target | **Amateur-dev friendly**. Prefer boring, reliable systems over advanced combat architecture. |
| Platform Priority | **Undocumented**. Controls must remain simple enough for keyboard/controller/mobile-style mapping. |
| Camera / Play Plane | **2D side-scrolling combat plane**. |
| Tone | Fast, flashy, supernatural, arcade-like, lightweight rogue-lite pressure. |
| Story Intent | TBD. Story should support combat flavor without forcing complex quest or narrative systems. |

### Experience Pillars

1. **Fast Basic Action**
   The player should move, attack, dodge, use skills, and swap characters with little friction.

2. **Simple Team Rotation**
   Team play is about spending available skills and buffs, not building long combo routes.

3. **VFX-First Impact**
   Combat should feel powerful through hit-stop, screen shake, slash arcs, particles, sound, and damage numbers more than complex character animation.

4. **Rogue-Lite Stat Growth**
   Run upgrades should mostly increase numbers or simple effects. They should not unlock complicated new action rules.

5. **Low Complexity Always Wins**
   The correct solution is usually the easiest one to implement, tune, and debug.

---

<!-- @tag:visual-audio -->
## Visual, Mood, and Audio Direction

### Mood Board Keywords

- 2D side-scrolling dungeon action
- Flashy supernatural slashes and impacts
- Fast room clears
- Readable boss telegraphs
- Hollow Knight-like boss readability
- Dungeon Slasher / Skul-like action pacing

### Art Direction

Neo-JJK should prioritize readable silhouettes, large hit VFX, strong attack shapes, and simple enemy tells. Character animation can be short and economical as long as the VFX sells the hit.

Avoid animation requirements that demand precise martial-arts readability, frame-perfect cancel points, or long hand-authored combo strings.

### Combat Presentation Rules

- Use large slash arcs, burst circles, impact flashes, hit-stop, and camera shake to sell attacks.
- Keep enemy danger zones visually clear.
- Prefer simple wind-up, active, and recovery presentation over detailed frame-data logic.
- Boss attacks should telegraph through clear shapes, warning zones, projectiles, and arena patterns.

### Audio Direction

Sound should make combat easy to read:

- Basic hits need short, punchy feedback.
- Skills and ultimates need distinct charge/release sounds.
- Swap actions need a quick identity cue.
- Boss danger tells should be audible before the attack becomes active.

---

<!-- @tag:platform-input -->
## Platform and Input Philosophy

### Primary Rule

The current platform target is undocumented, so the design must stay input-light.

All combat should fit a small action layout:

- Move
- Basic attack
- Dodge / dash
- Skill
- Ultimate
- Swap character
- Interact, if needed

### Input Rules

- No fighting-game motion inputs.
- No strict link timing.
- No frame-perfect cancel requirements.
- No deep directional combo trees.
- No required air-combat execution.

### Jump / Air Rule

Jumping and light traversal are TBD. If jump exists, it should not become a full aerial combat system.

Removed from the active design:

- Air attack as a core combat layer
- Plunge attack
- Real juggle routes
- Knockdown combo extensions

---

<!-- @tag:core-loop -->
## Core Game Loop

The fundamental rogue-lite loop is:

1. Choose a team, character loadout, or starting modifier.
2. Enter a side-scrolling combat room.
3. Clear enemies using basic attacks, skills, ultimates, dodges, and simple swaps.
4. Pick a reward, potential, stat boost, or effect upgrade.
5. Continue through more rooms, events, elites, and bosses.
6. Win the run or fail, then return to the run start/hub flow.

### Moment-To-Moment Combat Loop

The active combat rhythm is:

1. Approach enemies and basic attack.
2. Use a character skill or ultimate when ready.
3. Swap to a buffer/support character.
4. Apply a buff, debuff, shield, heal, or setup effect.
5. Swap back to the DPS character.
6. Spend burst damage.
7. Dodge hazards and repeat.

This is similar to a simplified Genshin-style rotation:

**DPS -> spend skill/ult -> swap buffer/support -> apply buff -> swap DPS -> burst -> swap back -> repeat**

### What This Replaces

The old micro loop of **Defensive <-> Offensive** pressure is removed.

Neo-JJK should use a generic rogue-lite action loop instead:

- Enter room
- Kill enemies
- Avoid hazards
- Collect reward
- Improve build
- Fight boss

### Progression Cadence

Difficulty should ramp through enemy count, projectile density, hazard patterns, elite modifiers, and boss phases.

Do not ramp difficulty through execution-heavy combos, frame traps, or enemy systems that require fighting-game knowledge.

### Failure / Recovery Pattern

Players should fail because they mistimed dodges, stood in boss patterns, chose risky upgrades, or got overwhelmed by room pressure.

Players should not fail because they missed strict cancels, optimal combo routes, perfect swaps, or hidden frame rules.

---

<!-- @tag:team-combat -->
## Team Combat Direction

### Core Rule

Team combat is **rotation-based**, not combo-driven.

Characters are tools in a simple loop:

- DPS stays active and deals most damage.
- Buffer/support swaps in briefly.
- Buffer/support spends skill or ultimate.
- DPS returns to use stronger damage.

### Swap Rules

Allowed:

- Swap character during combat.
- Swap during or after basic attacks if implementation is simple.
- Optional swap-in effect, such as small damage, shield, buff, or particle burst.

Not allowed:

- Perfect Swap from Zenless Zone Zero.
- Swap parry.
- Swap counter.
- Swap-specific slow motion timing windows.
- Swap routes that require lab-style combo practice.
- Team swap systems that require complex animation cancel logic.

### Character Kit Budget

Each character should stay small:

| Kit Part | Budget |
| --- | --- |
| Basic Attack | 2-3 simple states/hits maximum |
| Skill | 1 simple active skill |
| Ultimate | 1 flashy high-impact action |
| Passive | Simple stat/effect rule |
| Swap Effect | Optional, simple, non-precision |

Characters should feel different through:

- Range
- Cooldown
- Area shape
- Damage type
- Buff/debuff role
- VFX identity
- Simple status effects

Characters should not feel different through:

- Long combo lists
- Stance routing
- Air routes
- Grab routes
- Frame advantage
- Character-specific cancel rules

---

<!-- @tag:combat-scope -->
## Combat Scope and Removed Mechanics

### Active Combat Model

Neo-JJK combat is:

- Fast-paced
- Side-scrolling
- Hack-and-slash
- Skill cooldown based
- Ultimate burst based
- Team-rotation based
- VFX-heavy
- Simple to tune

### Removed Entirely

The following are not part of the active design:

- Fighting-game DNA as a system goal
- KOF / BlazBlue-style combo routing
- Frame data rules
- Frame advantage logic
- Strict cancel policy
- Link timing
- Grab / throw sessions
- Air attack systems
- Plunge attack systems
- Real knock-up juggle systems
- Real knock-fall combo sessions
- Complex hit reaction trees
- Defensive/offensive stance micro loop
- ZZZ-like Perfect Swap
- Combo-driven character potential upgrades

### Hit Reaction Policy

Allowed:

- Hit-stop
- Short stagger
- Knockback
- Fake knock-up animation
- Fake knockdown animation
- Boss flinch VFX for feedback

Not allowed:

- Real juggle state machines
- Extended airborne combo rules
- Enemy launch physics as a core system
- Boss stagger-lock loops
- Combo counter requirements for damage

Hit reactions should be presentation-first. If the player sees a knock-up, it can simply be an animation/VFX trick rather than a deep gameplay state.

---

<!-- @tag:progression -->
## Rogue-Lite Progression and Character Potential

### Character Potential Rule

Character Potential now acts like generic rogue-lite growth.

Potential may:

- Increase stats
- Increase effect strength
- Reduce cooldown
- Increase skill area
- Add a simple projectile
- Add a simple status chance
- Improve shield/heal/buff value
- Improve ultimate charge or damage

Potential must not:

- Add new combo routes
- Add new cancel rules
- Add new air actions
- Add new input strings
- Change the character into a complex fighting-game kit

### Upgrade Design Rules

Good upgrade examples:

- Basic attack damage +15%
- Skill cooldown -10%
- Ultimate damage +20%
- Dash leaves a small damage trail
- Buff duration +2 seconds
- Skill area becomes wider
- First hit after swap deals bonus damage
- Gain shield when using ultimate

Bad upgrade examples:

- Unlock an extra launcher branch
- Enable jump cancel after hit 2
- Add a perfect-swap counter route
- Add frame advantage bonuses
- Add combo-only damage scaling
- Require a strict input sequence

### Reward Categories

| Reward Type | Role | Intended Effect |
| --- | --- | --- |
| Stat Boost | Simple power growth | Increase damage, health, crit, defense, speed, or cooldown efficiency. |
| Skill Modifier | Light build identity | Change area, projectile count, duration, or status chance. |
| Team Modifier | Rotation support | Improve buffs, swap effects, energy gain, or support uptime. |
| Survival Modifier | Forgiveness | Add shield, heal, damage reduction, revive, or hazard resistance. |
| Economy Modifier | Run pacing | Increase currency, reward choices, rerolls, or upgrade quality. |

---

<!-- @tag:items -->
## Canonical Gameplay Content - Items and Upgrades

Items should be understandable in one sentence. Avoid hidden formulas and combo conditions.

| Item / Upgrade Type | Role | Intended Effect |
| --- | --- | --- |
| Damage Charm | DPS scaling | Increase basic, skill, or ultimate damage. |
| Cooldown Charm | Skill uptime | Reduce cooldowns or refund cooldown after room clear. |
| Shield Charm | Survival | Give shield on swap, skill use, or boss phase transition. |
| Burst Charm | Ultimate support | Increase ultimate damage or energy gain. |
| Status Charm | Simple synergy | Add burn, slow, bleed, curse, or other straightforward status effects. |
| Reward Charm | Rogue-lite pacing | Add rerolls, extra choices, or increased currency. |

---

<!-- @tag:enemies -->
## Canonical Gameplay Content - Enemies and Threats

### Trash Enemy

**Role:** Basic target for fast room-clearing.

**Design intent**
- Dies quickly.
- Teaches attack range and crowd control.
- Can stagger or knock back lightly.

**Counterplay**
- Basic attacks, skill area damage, and simple dodging.

### Ranged Enemy

**Role:** Forces movement and target priority.

**Design intent**
- Shoots readable projectiles.
- Stays behind melee enemies when possible.
- Low health, moderate danger.

**Counterplay**
- Dash through projectiles, close distance, or clear with area skills.

### Shield / Guard Enemy

**Role:** Slows down button-mashing without requiring complex counters.

**Design intent**
- Blocks or reduces frontal damage.
- Vulnerable after attacking or from behind.
- Should not require parry systems.

**Counterplay**
- Dodge around, use skill area damage, or swap into burst.

### Elite Enemy

**Role:** Adds mini-boss pressure inside rooms.

**Design intent**
- Has 2-3 clear attacks.
- Uses larger telegraphs than trash enemies.
- Drops stronger rewards.

**Counterplay**
- Respect telegraphs, dodge, burst during recovery.

### Hazard / Projectile Pattern

**Role:** Adds rogue-lite room pressure.

**Design intent**
- Creates simple Touhou-lite avoidance moments.
- Uses clear danger indicators.
- Does not require pixel-perfect dodging.

**Counterplay**
- Move, dash, reposition, kill the source.

---

<!-- @tag:bosses -->
## Boss Battle Direction

### Boss Fantasy

Bosses are no longer traditional fighting-game opponents.

They should feel closer to **generic 2D action bosses**:

- Large body
- Strong silhouettes
- Clear attack tells
- Big area attacks
- Bullet/projectile patterns
- Phase changes
- Short punish windows
- High VFX spectacle

Hollow Knight is a better boss reference than KOF, BlazBlue, or ZZZ.

### Boss Rules

Bosses should:

- Resist stagger-lock.
- Ignore real juggle logic.
- Use large readable AOE attacks.
- Use projectile or pattern pressure.
- Force dodging and repositioning.
- Give obvious openings after major attacks.

Bosses should not:

- Be combo dummies.
- Be thrown or grabbed.
- Be launched into extended air combos.
- Require perfect swap reactions.
- Require frame-trap knowledge.
- Depend on player combo optimization to be fun.

### Boss Encounter Loop

1. Boss telegraphs.
2. Player dodges or repositions.
3. Boss recovery creates a damage window.
4. Player uses DPS, support buff, skill, ultimate, or swap burst.
5. Boss changes pattern or phase.
6. Repeat with increasing hazard density.

---

<!-- @tag:mechanics -->
## System Mechanics

### Basic Attack

- 2-3 hit loop maximum.
- Simple forward-facing hitboxes.
- Can be interrupted only if implementation remains simple.
- Damage and VFX matter more than animation depth.

### Skill

- One active skill per character.
- Cooldown-based.
- Should have obvious area, projectile, buff, shield, or movement purpose.
- Avoid alternate branches and strict timing.

### Ultimate

- Flashy burst action.
- High impact VFX.
- Simple resource or cooldown gate.
- Should be easy to trigger and understand.

### Dodge / Dash

- Simple survival verb.
- Can include short invulnerability if easy to implement and tune.
- Should be readable and responsive.

### Swap

- Changes active character.
- May have cooldown if needed.
- May trigger a simple swap effect.
- Does not create perfect-swap gameplay.

### Buff / Debuff

- Use simple timers and stat modifiers.
- Avoid stacking rules that need spreadsheets.
- Make UI feedback readable.

---

<!-- @tag:inspirations -->
## Inspirations and Design Boundaries

### Primary Inspirations

- **Dungeon Slasher** - fast side-scrolling action, room pressure, simple combat readability.
- **Oblivion Override** - action rogue-lite pacing and quick build growth.
- **Skul: The Hero Slayer** - character identity through small kits and run upgrades.
- **Genshin Impact** - simplified team rotation model: DPS, support, buff, burst, repeat.
- **Hollow Knight** - boss readability, telegraphs, AOE patterns, punish windows.

### What To Borrow

- Fast room-clearing.
- Simple attack verbs.
- Upgrade choices that are easy to understand.
- Bosses with clear tells and pattern pressure.
- Team rotation based on skill cooldowns and buffs.
- Big VFX that makes simple attacks feel good.

### What Not To Copy Blindly

- KOF / BlazBlue execution complexity.
- Fighting-game frame data depth.
- ZZZ Perfect Swap timing and counter systems.
- Long combo trees.
- Advanced air juggling.
- Animation-heavy combat that amateur production cannot support.
- Upgrade systems that mutate simple characters into complex combo kits.

---

<!-- @tag:tech-stack -->
## Actual Project Stack and Repo Reality

> This section reflects what is documented in the currently loaded project context. Unknowns are intentionally marked unknown instead of invented.

### Technical Stack

| Area | Current Repo Truth |
| --- | --- |
| Engine | **Undocumented in current context** |
| Render Pipeline | **Undocumented in current context** |
| Gameplay Dimension | **Target: 2D side-scrolling** |
| Input | **Undocumented in current context** |
| Camera | **Target: side-view camera** |
| UI | **Undocumented in current context** |
| Networking | **Undocumented in current context; assume none unless project files prove otherwise** |
| Build Scenes | **Undocumented in current context** |

### Implementation Bias

When coding later, choose simple systems:

- Data-driven character definitions where practical.
- Cooldown timers over complex combo states.
- Stat modifier lists over custom upgrade logic per item.
- Simple enemy state machines.
- Clear VFX hooks.
- Minimal inheritance.
- Minimal global manager growth.

---

<!-- @tag:prototype-truth -->
## Current Prototype Truth

> No runtime source files were inspected while writing this Neo GDD. Future implementation tasks must inspect the actual project files before making code changes.

The current design direction is authoritative, but the current implementation status is unknown from the loaded context.

### Existing Runtime Shape

Undocumented. Before implementation, inspect the actual project folders and identify:

- Player controller
- Character data
- Attack / skill logic
- Enemy logic
- Room or level flow
- UI
- Camera
- VFX and audio hooks

### Current Prototype Debt

Known from design direction:

- The previous GDD direction is no longer the active target.
- Any fighting-game or perfect-swap assumptions must be removed from future planning.
- The Neo GDD needs future updates after actual code inspection.

Unknown until code inspection:

- Scene structure
- Existing scripts
- Existing combat implementation
- Build settings
- Current input system

---

<!-- @tag:scene-roles -->
## Canonical Future Scene Roles

Scene names are TBD. These are target roles, not confirmed build settings.

| Scene Role | Purpose |
| --- | --- |
| `MainMenu` | Start game, settings, run entry. |
| `RunStart` / `Hub` | Select team, starting modifiers, and run setup. |
| `CombatRoom` | Standard side-scrolling room clear gameplay. |
| `RewardRoom` | Choose upgrade, heal, shop, or event reward. |
| `BossRoom` | Pattern-based boss fight. |
| `Result` | Win/fail summary and return to run flow. |

---

<!-- @tag:system-ownership -->
## Canonical Future System Ownership

| System | Ownership |
| --- | --- |
| `PlayerController` | Movement, dodge, active character routing, player input. |
| `CharacterRuntime` | Current character stats, cooldowns, skill/ultimate use, active buffs. |
| `TeamController` | Character swap, team roster, shared team resources if any. |
| `AttackResolver` | Hit detection, damage application, hit-stop, simple knockback/stagger. |
| `UpgradeSystem` | Run rewards, potentials, stat/effect modifiers. |
| `EnemyController` | Enemy movement, attacks, health, simple state behavior. |
| `BossController` | Boss phases, telegraphs, AOE/projectile pattern sequencing. |
| `RoomFlowController` | Room start, spawn waves, clear condition, reward transition. |
| `VfxAudioPresenter` | Combat feedback, impact effects, skill/ultimate presentation. |
| `UICombatHud` | Health, cooldown, ultimate, swap, reward choices. |

### Hard Rule

Do not rebuild fighting-game architecture unless a human explicitly changes the design direction again.

Do not dump all future gameplay into one global script just because it is faster today. Keep systems small, but do not over-engineer them.

---

<!-- @tag:architecture -->
## Target Architecture Direction

### Architecture Principles

1. **Simple combat verbs first**
   Build basic attack, skill, ultimate, dodge, and swap before adding any extra mechanics.

2. **Data-driven enough, not data-driven forever**
   Character stats, cooldowns, VFX references, enemy stats, and upgrades should be authorable data when practical. Do not build a giant toolchain before the game loop works.

3. **Presentation reacts to gameplay**
   VFX, SFX, animation, camera shake, damage numbers, and hit-stop should respond to simple gameplay events.

4. **Cooldowns and modifiers over combos**
   Complexity should live in stats and simple effects, not in input timing or combo state.

5. **Readable boss patterns**
   Boss logic should be pattern/phase based, not fighting-game AI based.

6. **Amateur-friendly maintenance**
   A future tired developer should understand a feature quickly.

### Data-Driven Content Targets

- `CharacterDefinition` - health, movement, attack data, skill, ultimate, role, VFX/audio references.
- `SkillDefinition` - cooldown, damage, area, effect, status, VFX/audio.
- `UpgradeDefinition` - reward text, stat modifiers, simple triggered effects.
- `EnemyDefinition` - health, movement, attack list, reward value.
- `BossPatternDefinition` - telegraph, attack shape, projectile/area pattern, recovery.

### Avoid Overbuilding

Do not create:

- Full fighting-game frame data tables.
- Deep combo scripting languages.
- Advanced cancel graphs.
- Real-time character action editors.
- Custom physics juggle stacks.
- Perfect-swap state machines.

---

<!-- @tag:guardrails -->
## Contributor Guardrails

### Preserve These Non-Negotiables

- Neo-JJK is a **2D side-scrolling action hack-and-slash rogue-lite**.
- Combat is **rotation-based**, not combo-driven.
- Team swap is simple character swapping, not Perfect Swap.
- Character kits are small.
- Upgrades increase stats/effects, not moveset complexity.
- Bosses are pattern-based action bosses, not fighting-game opponents.
- Low development effort is a design requirement.

### You May Refactor Aggressively

- Old combo assumptions.
- Fighting-game terminology in future specs.
- Perfect-swap planning.
- Character potential that expands movesets.
- Complex defensive/offensive loop descriptions.

### Do Not Do These

- Reintroduce frame-data rules as gameplay requirements.
- Add grab/throw systems.
- Add air/plunge combat systems.
- Add real juggle combo systems.
- Build complex cancel logic.
- Add ZZZ-like swap timing systems.
- Make bosses stagger-lockable combo targets.
- Create upgrades that require combo mastery.

### AI Safety Rule

Every future implementation must clearly separate:

- **Current implementation truth**, confirmed by inspected files.
- **Target design direction**, defined by this Neo GDD.

Do not hallucinate completed architecture, engine details, scene names, or implemented systems.

---

<!-- @tag:roadmap -->
## Phased Roadmap

### Phase 1 - Lock the Simple Combat Prototype

**Goal:** Prove the basic action loop with one playable character and one enemy type.

- Movement
- Basic attack loop
- Skill
- Ultimate
- Dodge
- Hit-stop / stagger / knockback
- Basic VFX and SFX feedback

### Phase 2 - Add Team Rotation

**Goal:** Prove simple swapping without perfect-swap complexity.

- 2-3 character team
- Swap button
- Cooldowns per character
- Simple support/buffer skill
- DPS -> support -> DPS loop
- Minimal HUD for active character, cooldowns, and ultimate

### Phase 3 - Add Rogue-Lite Rewards

**Goal:** Make repeated rooms feel different through simple upgrades.

- Reward choices after room clear
- Stat boosts
- Skill modifiers
- Team/swap modifiers
- Survival upgrades
- Basic economy/reroll if needed

### Phase 4 - Add Boss Pattern Slice

**Goal:** Prove the new boss direction.

- Large boss
- Clear telegraphs
- AOE/projectile patterns
- Phase change
- Damage windows
- No stagger-lock or juggle dependence

### Phase 5 - Expand Content

**Goal:** Scale only after the simple loop works.

- More characters
- More enemy archetypes
- More upgrades
- More rooms
- More bosses
- Better VFX/audio polish

---

## Final Instruction To Contributors

Neo-JJK is not trying to be a fighting game.

The correct target is:

- **Fast 2D side-scrolling hack-and-slash**
- **Simple team skill rotation**
- **Generic rogue-lite room and reward loop**
- **Flashy VFX-driven combat feel**
- **Low execution burden**
- **Low implementation burden**

When deciding between a cool complex mechanic and a simple readable mechanic, choose the simple readable mechanic.

When deciding between animation depth and VFX impact, choose VFX impact.

When deciding between combo expression and rogue-lite stat/effect growth, choose rogue-lite stat/effect growth.

Build the version an amateur developer can actually finish.
