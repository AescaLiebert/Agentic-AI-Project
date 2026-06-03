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

This Design aim for Casual Game with Low Floor. Low Ceiling. High Impact in Flashy visuals.

---

<!-- @tag:identity -->
## Project Identity

| Field | Direction |
| --- | --- |
| **Working Title** | **Neo-JJK** |
| **Genre** | **2D Side-Scrolling / Action / Hack-and-Slash / Rogue-lite** |
| **Core Fantasy** | Clear chaotic side-scrolling combat rooms with a small team of flashy characters, swapping them to spend skills, apply buffs, and burst enemies down. |
| **Player Promise** | Fast basic action, readable attacks, big VFX, simple upgrades, and easy team rotations. |
| **Game Design Philosophy** | **Casual low floor and low ceiling. fast-paced reward and execution.** |
| **Development Philosophy** | Highly Systematic and Architecture like senior programmer Clean Code. Prefer simple, reliable systems rather than hard-coded. The codebase should short and modular to prevent Bug. |
| **Platform Priority** | PC-Focus (Keyboard/Controller) |
| **Camera / Play Plane** | 2D side-scrolling combat plane. |
| **Tone** | Fast, flashy, supernatural, arcade-like, lightweight rogue-lite pressure. |
| **Story Intent** | "Shin" died and was transported into Death Game Cursed Realm by Spirit Waifu name "Blind" and discovered that all players contain a Cursed Technique called "Aura" that amplify their fighting spirit to fight Monster "Fiend". All of you are rival and your enemy is your time. No matter you died in Curse Realm you will respawn at the beginning but only  1 out of 50 Players can survive and brought back to life so if you are getting back behind, you will forget who you are and become a one of "Fiend". |

### Experience Pillars

1. **Fast-paced Combat Hack & Slash**
   - The gameplay prioritize **Moment-to-Moment Action and Flashy Visuals**.
   - **Simple Learning Curve** : No character reqiured much learn to mastery. High skill come from Team-Building focus

2. **Beat Them and Acquire Them**
   - **Defeat Bosses**: Defeating other players (bosses) in the Cursed Realm unlocks them as playable characters in your pool.
   - **Soul Shop**: Shop for selling what you get from previously run (like *Dead Cell*) such as Character, Artifact, Items etc and Purchase and recruit defeated players to build your team.
   <img src="https://digitalchumps.com/wp-content/uploads/2024/01/Jin.gif?x96629" alt="Blazblue Entropy Effect Combat]" width="800">

3. **Rogue-lite Mechanics**
   - **Soul System (Character Potential)**: Upgradable character progression that unlocks new combo routes, modifies passive loops, and changes skill utilities.
   - **Artifact System**: Passive and active power-ups found during Cursed Realm runs.

---

<!-- @tag:visual-audio -->
## Visual, Mood, and Audio Direction

### Mood Board Keywords

<img src="https://i.redd.it/p4scnbqwrp991.gif" alt="Concept art" width="800">

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

**PC Keyboard & Mouse is the authoritative input model.** Gamepad controller support is a mapped port layer mirroring the exact same verb set and timing windows, without redesigning combat around precision differences.

### PC Keyboard & Mouse Controls

| Input | Combat Verb | Description / Behavior |
| --- | --- | --- |
| **A / D** | Move Left / Right | Horizontal locomotion. Dash cancels can be directed by holding A or D. |
| **Space** | Jump | Double jump supported. Press in air to double jump. |
| **LMB** | Basic Attack (BA) | Performs state-based basic attack chains. |
| **RMB** | Skill | Triggers character-specific skill. |
| **Shift** | Dash | Invincibility-frame dash. |
| **Q** | Ultimate | Triggers character ultimate. |
| **S (on platform)** | Drop down | Drop down to the platform below. |
| **1, 2, 3** | Team Swap | Swaps active character to slot 1, 2, or 3. |

### Input Rules

1. **Buffer Window**: The system maintains a **12-frame input buffer**. Inputs registered within 12 frames of an active action ending are queued and executed on the first possible frame.
2. **Priority Input Type over Input Order** : Input Type Hierarchy is **"System/Swap/Reaction" -> "Skill/Ultimate" -> "Dash/Jump" -> "Basic Attack" -> "Basic Movement"**. If the player presses different input type in the buffer window, the input type with higher priority will be executed and clear the cache.
3. **Team Swap** : When press Team Swap will clone another character into field with **Swap animation sequence** while the previous character will continue to execute their last action until end then disappear(this called **Team Swap Cancel**) and new character will Inheritance the last Input buffer of previous character to continue chaining.

---

<!-- @tag:core-loop -->
## Core Game Loop

<img src="https://digitalchumps.com/wp-content/uploads/2024/01/Gorgeous-action-packed-battles.gif?x96629" alt="Blazblue Entropy Effect Combat]" width="800">

1. Choose a team, character loadout, or starting modifier.
2. Enter a side-scrolling combat room.
3. explore and clear enemies.
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

### Progression Cadence

- Run Scaling: Enemy health, speed, and aggression scale over time.
- Level Exploration: Explore more rooms as you progress into game and beat the new and more challenging bosses.
- Level Selection : Player can Choose Normal or Nightmare mode to exponential the difficulty and more punishing to player.

### Failure / Recovery Pattern

Players should fail because they mistimed dodges, stood in boss patterns, chose risky upgrades, or got overwhelmed by room pressure.

---

<!-- @tag:combat -->
## Combat Direction

### Team-Combat Rules

Team combat is **rotation-based**, not combo-driven.

Characters are tools in a simple loop:

- DPS stays active and deals most damage.
- Buffer/support swaps in briefly.
- Buffer/support spends skill or ultimate.
- DPS returns to use stronger damage.

**Role Classification**:

- **Main DPS**: High on-field presence, executes core combos and sustained loops.
- **Sub DPS**: Quick-swap burst, dumps Skills/Ultimates.
- **Support**: Defensive utility (shields/heals) and team-wide offensive buffs.

### Swap Rules

Allowed:

- **Team Swap Cancel** : When press Team Swap will clone another character into field with **Swap animation sequence** while the previous character will continue to execute their last action until end then disappear, the movement of the previous character will also be preserved and executed normally.
- **Team Swap Inheritance** : the new character will Inheritance the last Input buffer of previous character to continue chaining.
- **Optional** swap-in effect, such as small damage, shield, heal, buff, or particle burst depend on Character Passive or Artifact.

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
<!-- @tag:characters -->
### Character Kit Design Principles

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

### Combat Action System

<img src="https://wutheringlab.com/wp-content/uploads/2023/06/8.gif" alt="Wuthering Wave Combat]" width="800">
Every character has actions governed by distinct Wind-up (SF), Active (AF), and Recovery (RF) animation frames:

- **Basic Attack (BA)**: Simple Linear State Attack 2-3 State Hit like MMORPG. Fast and Loopable.
- **Skill**: Custom ability. Cancels BA. Cooldown-gated. A/D inputs steer/alter trajectory or form.
- **Ultimate (Ult)**: High-damage finisher. Cancels Skill.

All Action can be done on Ground or Air, The Player FSM should stay the same result.

Remove any Character Lock position/gravity on Action (Except Dash/Jump) to prevent character stuck loop.

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

### Character Data

#### Character Stats

<img src="https://upload-os-bbs.hoyolab.com/upload/2025/01/28/13693861/d7cf74b844c6ff690f37810e2610c852_2399325505013166081.jpg?x-oss-process=image%2Fresize%2Cs_1000%2Fauto-orient%2C0%2Finterlace%2C1%2Fformat%2Cwebp%2Fquality%2Cq_70" alt="Wuthering Wave Combat]" width="800">

| Stats | Value | Description |
| --- | --- | --- |
| ATK | 10 | Attack |
| DEF | 10 | Defense |
| HP | 100 | Health |
|SPD | X | Speed |
| CDR | XX% | Cooldown Reduction Mastery |
| CRIT | XX% | Critical Rate |
| CRIT Dmg | XX% | Critical Damage |
|DMG Mastery| XX% | Damage Mastery |
|DMG Res| XX% | Damage Resistance |

---

<!-- @tag:progression -->
## Rogue-Lite Progression

### Soul Shop System

- **Shop Lore** : a opportunist devil that collect a dead soul on Cursed Realm and sell for high price.
- **Shop Rule** : The Soul Shop act as a safe haven for players. It is the only place where players can change, replace, or purchase their loadout (Character/Artifact) before venturing into the next Cursed Realm.  
- **Shop Algorithm**: The shop algorithm will use the pool of character, artifact that the player have unlocked and the pool of enemy in the Cursed Realm that the player have defeated or explored. it never present what you never seen.
- **Shop Reroll**: Player can Reroll shop Item but the new Item price will overpriced increase every time you reroll.

### Utility Resource

| Entity | Description | Rogue-lite Reset |
| --- | --- | --- |
|Currency | The In-Game Utility Currency for **Soul Shop** | Lose |
| Soul Fragment | The Currency of the game can only acquire by defeating the Elite/Boss enemy on the Cursed Realm Run. | Retain |
| Character | The Characters that can use in the game. | Lose but retain the upgrade |
| Artifact | The Passive and active power-ups found during Cursed Realm runs. | Lose but Retain the Upgrade |
| Items | The consumable items that can use during the Cursed Realm runs. | Lose |

P.S Lose but retain Upgrade mean you lose that Item/Character/Artifact if you die in Cursed Realm Run but if you able to buy again, they will come with latest of your unlocked upgrade(e.g Instead of new Fresh Potential Lv.0 they come with Potential Lv.5 and Artifacts also the same rule.)

### Character Potential Rule

![Character Potential Rule](https://static.wikia.nocookie.net/gensin-impact/images/3/3f/Constellation_Menu.png/revision/latest?cb=20220411014019)

#### Permanent Potential Upgrade

- it represent a system like Constellation or Aetherium Core in Honkai Star Rail. this will use to permanent upgrade that character. Linear path no seperation, each upgrade will always benefit to the character in a different way.
- Potential upgrade only acquired when you buy that character from Soul Shop again, the price will increase every time you upgrade.
- Max upgrade is 1/6 step

#### Potential can

- Increase stats
- Increase effect strength
- Reduce cooldown
- Increase skill area
- Add a simple projectile
- Add a simple status chance
- Improve shield/heal/buff value
- Improve ultimate charge or damage

#### Potential must not

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
