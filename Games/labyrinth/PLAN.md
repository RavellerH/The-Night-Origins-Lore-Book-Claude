# Labyrinth — 2D Turn-Based RPG Plan

> A 2D exploration RPG with Golden Sun / Final Fantasy-style turn-based combat.
> **PROTOTYPE SCOPE:** Floor 1 only — Avery + Nate enter the Labyrinth portal from the City of the Blinding Light.
> **Art assets handled separately — this plan covers game mechanics, systems, and structure only.**

---

## Lore Foundation

**Source:** Book 1, Chapter 63 — The Labyrinth Portal / Underworld meeting.

After 2 years of training at the City of the Blinding Light, Avery leads Nate (and others) to the ruins at the edge of Vleyrra Forest. A ghostly figure at a dark purple tree creates a time-distortion portal. They are pulled inside — into a giant labyrinth with dark purple branches, cave-like ceiling, lava-lit sky, and demons everywhere. The labyrinth is a **Deepborn weak veil** — a crack in the ancient seals.

**PROTOTYPE MOMENT:** Avery and Nate step into the Labyrinth for the first time. They are 18. They have trained for 2 years. This is their first real combat test against Deepborn creatures. Avery must face her fear of her own crystal magic.

---

## PROTOTYPE SCOPE (v0.1 — Build This First)

### What's In
| Feature | Detail |
|---------|--------|
| **Characters** | Avery (Crystal Origin) + Nate (Light Mage) — 2-party |
| **Map** | Floor 1 — 10×10 tile single-screen map |
| **Enemies** | 3 types: Corrupted Imp (×2 patrol), Dark Creeper (×1), mini-boss: Demon Scout (×1) |
| **Combat** | Full turn-based battle: turn order, skills, items, defend, flee |
| **Skills** | Avery: Crystal Shard, Crystal Barrier. Nate: Light Bolt, Radiant Heal |
| **Items** | Healing Drop (×3), Crystal Shard consumable (×2) |
| **Exploration** | WASD movement, wall collision, chest (×1), stairs down (exit) |
| **UI** | HP/MP bars, action menu, damage numbers, turn queue display |
| **Save** | Single save slot at floor start |
| **Story beats** | Intro text on entry, 1 mid-floor dialogue, exit text |

### What's NOT In (Yet)
- Fran's chapter, Callen's epilogue, Chapters 2-3
- Floors 2-10, boss fight, escape sequence
- Myst, Lynne, Alyse, equipment system
- Bond Meter, combo skills, mini-map
- Multiple save slots, New Game+, audio
- Status effects beyond basic (corruption, slow)
- XP/leveling (prototype uses fixed stats)

### Why This Scope
- **Test the battle engine** — turn order, damage calc, skill system — with 2 characters
- **Test the map engine** — tile rendering, movement, encounters — on a small, debuggable map
- **One floor = one loop:** enter → explore → fight 3-4 battles → reach stairs → done
- **Expandable:** add floors, characters, skills by appending data — no architecture change needed
- **Playable in 5-10 minutes** — quick to test, quick to iterate

---

## Full Game Vision (Post-Prototype)

### Three Chapters, Converging at the End

| Chapter | Protagonist | Setting | Floors | Playtime |
|---------|------------|---------|--------|----------|
| 1: *Descent* | **Avery** (Party: Nate, Alyse) | Labyrinth upper levels — demon corridors | Floors 1-4 | ~45 min |
| 2: *The Other Path* | **Fran** (Party: Myst, Lynne) | Underworld mid-levels — time-distortion zones | Floors 5-7 | ~45 min |
| 3: *Convergence* | **Avery** (Party: Fran) | Deep levels — the castle at the bottom | Floors 8-10 + boss | ~60 min |

**Epilogue:** *The Exit* — brief surface-world scene as Callen (single combat encounter, narrative bridge).

Total playtime: ~2.5 hours for full playthrough.

---

## Core Gameplay Loop

```
Explore map → Encounter enemy → Turn-based combat → Gain XP/gold → 
Upgrade abilities → Explore next floor → Story beat → Repeat
```

### On the 2D Map (Exploration Phase)
- Player moves character(s) on a tile-based floor map
- Visible enemy sprites patrol paths (no random encounters)
- Touching an enemy → transition to battle screen
- Environmental hazards: corrupted zones (damage per step), broken bridges, locked doors
- Chests/pickups scattered on floors (consumables, key items)
- NPC waypoints: brief dialogue before/after certain floors
- Exit stairs/portal at floor end → next floor

### Battle Screen (Combat Phase)
- Separate battle scene with background matching current floor theme
- Party on left (2-3 characters), enemies on right (1-5 enemies)
- Turn order determined by Speed stat
- Each character acts on their turn: Attack, Skill, Item, Defend, Flee
- Victory → XP + gold + possible item drops
- Defeat → retry from last checkpoint (floor start)

---

## Character Roster

### Chapter 1: Avery's Party

#### Avery (Crystal Origin) — Player Character
| Stat | Base | Growth |
|------|------|--------|
| HP | 80 | +12/level |
| MP | 40 | +8/level |
| ATK | 8 | +2/level |
| DEF | 6 | +2/level |
| SPD | 7 | +1/level |
| MAG | 15 | +4/level |

**Skills:**
| Skill | MP Cost | Effect | Unlocked |
|-------|---------|--------|----------|
| Crystal Shard | 5 | Single-target magic damage | Floor 1 |
| Crystal Barrier | 8 | Ally DEF +50% for 3 turns | Floor 2 |
| Crystal Prison | 10 | Single-target damage + Slow | Floor 3 |
| Crystal Sphere | 20 | AoE damage to all enemies (ultimate) | Floor 4 |
| Purify | 12 | Remove corruption from ally / heal | Floor 5 |

**Weapon:** Crystal focus (no physical weapon — all magic)
**Weakness:** Low physical ATK, fragile early on
**Strength:** Highest MAG, party support via barriers

#### Nate (Light Mage)
| Stat | Base | Growth |
|------|------|--------|
| HP | 70 | +10/level |
| MP | 50 | +10/level |
| ATK | 6 | +1/level |
| DEF | 5 | +1/level |
| SPD | 8 | +2/level |
| MAG | 12 | +3/level |

**Skills:**
| Skill | MP Cost | Effect |
|-------|---------|--------|
| Light Bolt | 4 | Single-target light damage |
| Radiant Heal | 8 | Heal ally for MAG×2 HP |
| Blinding Flash | 6 | Lower enemy accuracy 1 turn |
| Aurora Wave | 15 | AoE light damage |

**Role:** Healer + secondary magic damage
**Synergy with Avery:** Light + Crystal = high sustained damage + healing backup

#### Alyse (Light Mage — Support Variant)
| Stat | Base | Growth |
|------|------|--------|
| HP | 60 | +8/level |
| MP | 55 | +12/level |
| ATK | 5 | +1/level |
| DEF | 4 | +1/level |
| SPD | 9 | +2/level |
| MAG | 10 | +3/level |

**Skills:**
| Skill | MP Cost | Effect |
|-------|---------|--------|
| Light Dart | 3 | Single-target light damage |
| Mend | 6 | Heal ally for MAG×1.5 HP |
| Ward | 8 | Shield: absorb next hit (ally) |
| Light Pulse | 12 | AoE heal + minor damage |

**Role:** Pure support/healer, lower damage than Nate
**Lore note:** Alyse gets severely injured in Book 1 Part 2 — she leaves the party after Chapter 1

---

### Chapter 2: Fran's Party

#### Fran (Origin from Earth) — Player Character
| Stat | Base | Growth |
|------|------|--------|
| HP | 90 | +14/level |
| MP | 30 | +5/level |
| ATK | 10 | +3/level |
| DEF | 7 | +2/level |
| SPD | 8 | +2/level |
| MAG | 13 | +3/level |

**Skills:**
| Skill | MP Cost | Effect | Unlocked |
|-------|---------|--------|----------|
| Origin Burst | 6 | Single-target raw damage (high) | Floor 5 |
| Soul Link | 10 | Share damage with Myst for 2 turns | Floor 5 |
| Memory Edge | 8 | ATK-based attack, ignores DEF | Floor 6 |
| The Final Bond | 25 | Mega damage + heal party (ultimate) | Floor 7 |
| Earth Instinct | 15 | Dodge next attack, counter with Origin Burst | Floor 7 |

**Weapon:** None — raw Origin energy
**Weakness:** Lower MP pool, skills are high-cost
**Strength:** Highest HP, highest single-target burst damage

#### Myst (Soul-bond Companion)
| Stat | Base | Growth |
|------|------|--------|
| HP | 50 | +8/level |
| MP | 40 | +8/level |
| ATK | 7 | +2/level |
| DEF | 5 | +1/level |
| SPD | 10 | +3/level |
| MAG | 11 | +3/level |

**Skills:**
| Skill | MP Cost | Effect |
|-------|---------|--------|
| Quick Strike | 3 | Fast single-target attack |
| Bond Sense | 5 | Detect hidden items / paths |
| Soul Shield | 8 | Protect Fran: take damage for her |
| Dual Pulse | 12 | Combo with Fran: double Origin Burst |

**Role:** Scout + support, enables Fran's ultimate abilities
**Unique mechanic:** Myst and Fran share a "Bond Meter" — certain abilities charge it, spending it enables combo attacks

#### Lynne (Origin Mentor)
| Stat | Base | Growth |
|------|------|--------|
| HP | 75 | +10/level |
| MP | 45 | +10/level |
| ATK | 8 | +2/level |
| DEF | 6 | +2/level |
| SPD | 7 | +1/level |
| MAG | 14 | +4/level |

**Skills:**
| Skill | MP Cost | Effect |
|-------|---------|--------|
| Origin Wave | 8 | AoE Origin damage |
| Origin Sight | 5 | Reveal enemy weakness for 3 turns |
| Aura Shield | 10 | Party-wide barrier (absorb damage) |
| Origin Truth | 20 | Massive single-target damage (reveals weakness × 3) |

**Role:** Magic damage + weakness exploitation + party defense
**Lore note:** Lynne identifies Avery and Fran as Origins — in-game, she teaches Fran how to use her power

---

### Chapter 3: Converged Party

#### Avery (Post-Labyrinth) — Same stats as Chapter 1, but higher level
- Starts at level equal to average of Chapter 1 end + 1
- All skills retained

#### Fran (Post-Meeting) — Same stats as Chapter 2
- Joins at current level
- All skills retained

**New Combo Skill (Avery + Fran):**
| Skill | MP Cost | Effect |
|-------|---------|--------|
| Origin Crystal | 30 | Avery's crystal + Fran's Origin = massive AoE damage + full party heal |

This is the "What?!" moment from the lore — two Origins confirming each other, weaponized.

---

### Epilogue: Callen

#### Callen (Mage Hunter) — Solo, Single Encounter
| Stat | Value |
|------|-------|
| HP | 200 (fixed) |
| MP | 60 (fixed) |
| ATK | 35 (fixed) |
| DEF | 25 (fixed) |
| SPD | 20 (fixed) |
| MAG | 15 (fixed) |

**Skills:** Elven Strike, Hunter's Edge, Thunder Slash
**Purpose:** Single climactic battle against a corrupted Deepborn commander. No level-up, no grinding — pure narrative beat. After victory, transition to Book 1 surface ending.

---

## Enemy Bestiary

### Floor Themes & Enemy Types

| Floor Range | Theme | Enemies |
|-------------|-------|---------|
| Floors 1-2 | Demon Corridors | Corrupted Imp, Demon Scout, Dark Creeper |
| Floors 3-4 | Deepborn Taint | Mindless Drone, Corrupted Wolf, Tainted Mage |
| Floors 5-6 | Time Distortion | Phase Wraith, Temporal Shade, Echo Demon |
| Floors 7-8 | Underworld Castle | Deepborn Guard, Corruption Knight, Void Sentinel |
| Floors 9-10 | The Heart | Deepborn Lieutenant, The Corruptor, Deep Lord Fragment |
| Boss | Deep Lord Fragment | Multi-phase boss encounter |

### Sample Enemy Stats

#### Corrupted Imp (Floor 1 basic)
| HP | ATK | DEF | MAG | SPD | Drops |
|----|-----|-----|-----|-----|-------|
| 25 | 6 | 3 | 4 | 5 | 3-5 gold, 10% Crystal Shard (consumable) |

**Abilities:** Scratch (single-target attack)
**Weakness:** Light magic (+50% damage)

#### Mindless Drone (Floor 3)
| HP | ATK | DEF | MAG | SPD | Drops |
|----|-----|-----|-----|-----|-------|
| 50 | 10 | 8 | 5 | 4 | 5-8 gold, 15% Purifying Drop |

**Abilities:** Consume (damage + small corruption on target)
**Weakness:** Crystal magic (+40% damage), Fire magic (+30%)

#### Phase Wraith (Floor 5)
| HP | ATK | DEF | MAG | SPD | Drops |
|----|-----|-----|-----|-----|-------|
| 65 | 12 | 6 | 14 | 12 | 8-12 gold, 20% Time Fragment |

**Abilities:** Phase Shift (dodge next attack), Temporal Drain (damage + reduce MP)
**Weakness:** Origin magic (+60% damage)

#### Deepborn Lieutenant (Floor 9 elite)
| HP | ATK | DEF | MAG | SPD | Drops |
|----|-----|-----|-----|-----|-------|
| 200 | 25 | 18 | 22 | 15 | 30-50 gold, Deepborn Core (key item) |

**Abilities:** Corruption Wave (AoE damage), Mind Rend (single-target, chance to silence), Summon Drone (call 1 Mindless Drone)
**Weakness:** Origin + Crystal combo (+70% damage)

### Boss: Deep Lord Fragment (Floor 10)

**Phase 1:** The Waking
- HP: 400, ATK: 20, DEF: 15, MAG: 25, SPD: 10
- Abilities: Dark Pulse (AoE), Corruption Seal (reduce party MP), Void Gaze (chance to stun)
- At 50% HP → Phase 2

**Phase 2:** The Rising
- HP: 300 (remaining), ATK: 28, DEF: 12, MAG: 30, SPD: 14
- Gains: Summon Lieutenant (once per battle), Deepborn Roar (AoE + fear)
- At 25% HP → Phase 3

**Phase 3:** The Breaking
- HP: 150 (remaining), ATK: 35, DEF: 8, MAG: 35, SPD: 18
- Gains: Reality Tear (massive single-target), Time Fracture (skip a character's turn)
- Defeated → escape sequence

**Strategy hint:** Origin Sight (Lynne) + Origin Crystal (Avery + Fran combo) is the intended counter.

---

## Experience & Leveling

### XP Table
| Level | XP Required | Stat Increase |
|-------|------------|---------------|
| 1 → 2 | 30 | Small boost to all stats |
| 2 → 3 | 60 | Small boost |
| 3 → 4 | 100 | Small boost + skill unlock (if applicable) |
| 4 → 5 | 150 | Medium boost |
| 5 → 6 | 220 | Medium boost + skill unlock |
| 6 → 7 | 300 | Medium boost |
| 7 → 8 | 400 | Large boost + skill unlock |
| 8 → 9 | 520 | Large boost |
| 9 → 10 | 660 | Large boost + skill unlock |
| 10+ | Scaling | Diminishing returns |

### XP Distribution
- Basic enemy: 10-20 XP
- Elite enemy: 30-50 XP
- Mini-boss: 80-120 XP
- Boss: 200+ XP
- XP split evenly among party members

### Chapter Start Levels
| Chapter | Starting Level | Reason |
|---------|---------------|--------|
| Ch. 1 (Avery) | Level 1 | First time in combat |
| Ch. 2 (Fran) | Level 4 | Already has Underworld experience |
| Ch. 3 (Avery + Fran) | Average of Ch. 1 end + 1 | Time has passed, growth |
| Epilogue (Callen) | Fixed stats | Single narrative encounter |

---

## Items & Economy

### Currency
- **Gold** — dropped by enemies, found in chests
- **Crystal Shards** — rare drops, used for permanent skill upgrades
- **Time Fragments** — Fran chapter only, used for bond abilities

### Consumables

| Item | Cost (gold) | Effect |
|------|------------|--------|
| Healing Drop | 10 | Restore 30 HP |
| Greater Healing Drop | 30 | Restore 80 HP |
| Crystal Shard (consumable) | 20 | Restore 20 MP |
| Purifying Drop | 25 | Remove corruption status |
| Light Candle | 15 | Restore 15 MP to Light mage |
| Origin Bloom | 50 | Restore 40 MP to Origin user |
| Ward Stone | 40 | Barrier: absorb next hit |

### Equipment (Minimal — Stat Boosters Only)

No traditional weapons or armor. Characters use innate abilities. Equipment slots are **Focus Items** that boost stats:

| Slot | Example Items | Effect |
|------|--------------|--------|
| Focus | Crystal Lens, Origin Gem | +MAG, +MP |
| Charm | Light Pendant, Soul Thread | +SPD, +DEF |
| Core | Deepborn Essence, Time Shard | Unique passive abilities |

Equipment found in chests or dropped by elites/bosses. Max 1 per slot per character.

---

## Status Effects

| Effect | Description | Duration |
|--------|------------|----------|
| **Corrupted** | Take damage each turn, reduced healing | 3 turns (removable with Purifying Drop / Purify skill) |
| **Slowed** | -50% SPD | 2 turns |
| **Silenced** | Cannot use skills | 1-2 turns |
| **Stunned** | Skip turn | 1 turn |
| **Shielded** | Absorb next hit (barrier) | Until consumed |
| **Focused** | +30% MAG next turn | 1 turn |
| **Weakened** | -30% ATK/DEF | 2 turns |
| **Bond Charged** | Fran+Myst combo meter +1 | Until spent |

---

## Turn-Based Combat System

### Turn Order
- Determined by SPD stat at battle start
- Displayed as a turn queue bar at top of screen
- Ties broken by random roll
- SPD buffs/debuffs adjust position in queue

### Action Menu (per character turn)

```
┌──────────────────────────────────────┐
│  AVERY'S TURN                         │
│                                      │
│  ⚔ Attack     ✦ Skill               │
│  ✧ Item       ◆ Defend              │
│  ↩ Flee                             │
└──────────────────────────────────────┘
```

- **Attack:** Basic physical/magic attack (uses MAG for magic characters)
- **Skill:** Open skill list (shows MP cost, target selection)
- **Item:** Use consumable from inventory
- **Defend:** Reduce incoming damage 50% this turn, +10% SPD next turn
- **Flee:** Attempt escape (70% base chance, fails vs bosses)

### Target Selection
- After choosing attack/skill, cursor highlights valid targets
- Single-target: pick one enemy/ally
- AoE: auto-hits all enemies/allies
- Self: caster only
- Confirm → animation plays → damage numbers → next turn

### Damage Calculation

```
Damage = (ATK or MAG) × Skill Multiplier - (Target DEF / 2)
```

- Minimum 1 damage always
- Weakness: ×1.5 damage
- Resistance: ×0.5 damage
- Critical hit: 5% chance, ×2 damage
- Defend: ×0.5 incoming

### Battle End Conditions
- **Victory:** All enemies defeated → XP + gold + drops
- **Defeat:** All party members HP = 0 → retry from floor checkpoint
- **Flee:** Party escapes → return to map, enemy still patrolling

---

## Map Exploration System

### Floor Layout
Each floor is a tile-based map (not procedurally generated — hand-designed):

```
Size: ~20x15 tiles per floor (adjustable)
Tile size: Canvas-based rendering
View: Top-down 2D (like classic Zelda / early Final Fantasy overworld)
Scrolling: Screen-locked (one screen per floor, or camera follows player)
```

### Tile Types

| Tile | Behavior |
|------|----------|
| Floor | Walkable |
| Wall | Blocked |
| Corrupted Zone | Walk = take 5 damage per step, accumulates Corrupted status |
| Chest | Interact → open, receive item |
| Stairs Down | Exit to next floor |
| Stairs Up | Return to previous floor (if unlocked) |
| Door (locked) | Requires key item from floor |
| NPC | Interact → dialogue |
| Hazard (lava spike, void rift) | Blocked or damage on touch |
| Save Point | Rest party, save game |

### Enemy Encounters on Map
- Enemies are visible sprites that patrol set paths
- Walking into enemy's sprite → battle starts
- Walking behind enemy → can avoid fight (stealth option)
- Some enemies chase player if in range
- Defeated enemies disappear from map until floor reset

### Mini-Map
- Top-right corner shows simplified floor overview
- Reveals explored areas only
- Shows: player position, stairs, save points, undiscovered chests (as "?")

---

## Story Integration

### Narrative Moments (Non-Combat)

| Location | Event |
|----------|-------|
| Floor 1 start | Brief intro: Avery, Nate, Alyse enter the ruins. The purple tree. Portal. |
| Floor 2 mid | Nate: "Something's wrong with time here." First realization. |
| Floor 3 end | Alyse separated briefly — reunion with corrupted status, Avery learns Purify. |
| Floor 4 end | Exit through top. Avery emerges alone. Time has passed differently. |
| Floor 5 start | Fran's POV: she's been navigating the Underworld separately. Myst and Lynne with her. |
| Floor 6 mid | Lynne teaches Fran about Origin power. Bond Sense reveals hidden path. |
| Floor 7 end | Fran senses another Origin nearby. "Someone else... like me?" |
| Floor 8 start | Converged: Avery enters from above, Fran from below. They meet. |
| Floor 8 mid | **THE MEETING:** "Her aura is the same as yours, Fran..." "What?!" |
| Floor 9 | Together, they push deeper. Combo abilities unlock. |
| Floor 10 | Boss: Deep Lord Fragment. Defeated. |
| Floor 10 end | Escape. Exit through the top. Surface. |
| Epilogue | Callen: single battle on surface. Transition to Book 1 ending. |

### Dialogue System
- Battles: brief combat quips (random, character-specific)
- Story moments: full dialogue blocks with character portraits
- Choices: 1-2 dialogue choices per chapter (cosmetic, affect quips only)

---

## Save System

```js
const SAVE_KEY = 'kalmirth_labyrinth';

// Save data shape:
{
  chapter: 1,           // current chapter
  floor: 3,             // current floor
  party: [...],         // character levels, stats, skills, equipment
  inventory: [...],     // consumables, key items
  gold: 150,
  crystalShards: 2,
  exploredFloors: [1,2,3], // which floors have been fully explored
  bossDefeated: false,
  playtime: 3600,       // seconds
  checkpoint: { ... }   // last save point state
}
```

- Save at save points on floors
- Auto-save at chapter transitions
- 3 save slots
- localStorage persistence

---

## UI Layout

### Exploration Screen
```
┌──────────────────────────────────────────┐
│  FLOOR 3 — Deepborn Taint    Ch. 1      │
│                                          │
│                                          │
│            [ 2D MAP CANVAS ]             │
│                                          │
│                                          │
│                                          │
├──────────────────────────────────────────┤
│  HP: ████░░ 60/80   MP: ██░░░ 20/40     │
│  Gold: 45   Shards: 1                    │
│  [Nate: HP ███░░  Alyse: HP ████░]      │
└──────────────────────────────────────────┘
```

### Battle Screen
```
┌──────────────────────────────────────────┐
│  TURN QUEUE: Avery → Imp → Nate → Imp   │
├──────────────────────┬───────────────────┤
│                      │                   │
│   [Avery sprite]     │   [Imp sprite]    │
│   HP: ████░░ 60/80   │   HP: ██░░ 15/25  │
│   MP: ██░░░ 20/40    │                   │
│                      │                   │
│   [Nate sprite]      │   [Imp sprite]    │
│   HP: ███░░ 50/70    │   HP: ███░░ 20/25 │
│   MP: ████░ 40/50    │                   │
│                      │                   │
├──────────────────────┴───────────────────┤
│  ⚔ Attack    ✦ Skill    ✧ Item          │
│  ◆ Defend    ↩ Flee                      │
│                                          │
│  "Avery uses Crystal Shard!" → damage    │
└──────────────────────────────────────────┘
```

---

## Technical Architecture

### Single HTML File (matching existing game pattern)
```
Games/labyrinth/index.html
├── <style>     — All CSS (UI, battle screen, exploration, animations)
├── <body>      — Game canvas + UI overlay elements
└── <script>
    ├── GAME_DATA — character definitions, enemy definitions, skills
    ├── FLOOR_MAPS — tile data for each floor
    ├── STORY_BEATS — dialogue, narrative triggers
    │
    ├── Core Systems:
    │   ├── GameState — current state machine (explore/battle/story/menu)
    │   ├── Party — character management, stats, leveling
    │   ├── Inventory — items, consumables, equipment
    │   ├── CombatEngine — turn order, damage calc, status effects
    │   ├── MapEngine — tile rendering, movement, encounters
    │   ├── StoryEngine — dialogue, cutscenes, chapter transitions
    │   └── SaveSystem — localStorage save/load
    │
    ├── Rendering:
    │   ├── CanvasRenderer — 2D sprite rendering
    │   ├── BattleRenderer — battle screen layout
    │   └── UIRenderer — HUD, menus, dialogue boxes
    │
    └── Input:
        ├── Keyboard controls (WASD/arrows for movement)
        ├── Mouse/keyboard for menus
        └── Enter/Space for dialogue advance
```

### Controls

| Input | Exploration | Battle | Menus |
|-------|------------|--------|-------|
| WASD / Arrows | Move character | N/A | Navigate |
| Enter / Space | Interact | Confirm action | Confirm |
| ESC | Pause/menu | Cancel/go back | Cancel |
| I | Open inventory | Quick-item menu | — |
| C | Open character screen | — | — |
| M | Toggle mini-map | — | — |

---

## Visual & Audio Direction

### Color Palette (per floor theme)

| Floor | Background | Accent | Mood |
|-------|-----------|--------|------|
| 1-2 | Dark stone (#1a1a2e) | Purple glow (#8a4ab0) | Disorienting, new |
| 3-4 | Deepborn corruption (#1a0a1e) | Sickly green (#4a8a3a) | Unsettling |
| 5-6 | Time distortion (#0a0a2e) | Shifting blue/purple | Ethereal |
| 7-8 | Underworld castle (#2a1a1a) | Lava orange (#d06030) | Oppressive |
| 9-10 | The Heart (#0a000a) | Void black + red (#a02020) | Final, desperate |

### Audio (Optional — Can Be Phase 2)

| Context | Audio |
|---------|-------|
| Exploration (Floors 1-4) | Low ambient drone, distant drip sounds |
| Exploration (Floors 5-7) | Warped ambient, time-stretch sounds |
| Exploration (Floors 8-10) | Deep bass pulse, occasional whisper |
| Battle start | Sharp chime/impact |
| Basic attack | Whoosh + hit sound |
| Magic attack | Crystal chime / light hum |
| Victory | Short ascending tone |
| Defeat | Descending tone |
| Save | Soft pulse |
| Chapter transition | Portal whoosh |
| Boss fight | Intensified ambient + heartbeat |

---

## Implementation Order

### PHASE 0: PROTOTYPE (v0.1 — Build This First)

**Goal:** A playable 5-10 minute experience — Floor 1 of the Labyrinth with Avery + Nate.

| Step | Task | Detail |
|------|------|--------|
| 0.1 | **HTML skeleton** | Single `index.html` with `<canvas>`, CSS vars, basic layout |
| 0.2 | **Tile renderer** | Canvas draws 10×10 grid: floor tiles, wall tiles, chest, stairs |
| 0.3 | **Player movement** | WASD/arrow keys, wall collision, 2-char party (lead + follow) |
| 0.4 | **Enemy placement** | 3 enemy sprites on map with patrol paths |
| 0.5 | **Battle transition** | Touch enemy → fade to battle screen → return after victory |
| 0.6 | **Turn order system** | Queue bar: Avery → Enemy → Nate → Enemy (SPD-based) |
| 0.7 | **Action menu** | Attack, Skill, Item, Defend, Flee — keyboard navigation |
| 0.8 | **Skill execution** | Crystal Shard (Avery), Light Bolt (Nate) — damage calc + animation |
| 0.9 | **Healing** | Radiant Heal (Nate), Healing Drop item — restore HP |
| 0.10 | **Enemy AI** | Enemies use basic attack + 1 special ability |
| 0.11 | **Victory/defeat** | All enemies dead → XP + gold display. All party dead → retry. |
| 0.12 | **Chest + stairs** | Interact with chest → get item. Interact with stairs → exit screen |
| 0.13 | **Story text** | Intro on entry, mid-floor dialogue trigger, exit text |
| 0.14 | **HUD** | HP/MP bars for both characters, gold counter, turn queue |
| 0.15 | **Title screen** | "Labyrinth" title + "Start" + "How to Play" |

**Prototype deliverable:** Open `index.html` → Title → Start → Walk Floor 1 → 3-4 battles → Reach stairs → "Floor 1 complete" screen.

---

### PHASE 1: Chapter 1 Complete (The Descent)

1. **Floors 2-4 maps** — full tile layouts, enemy placement, chests.
2. **Narrative Triggers:**
   - **Floor 2:** Encounter **Frank** (Subjective time: 3 days).
   - **Floor 3:** Encounter **Alyse** (Corrupted status). Avery unlocks **Purify** skill.
   - **Floor 4:** Encounter **Ellen** (Subjective time: 1 week).
3. **Rune Fragment System:** Collectible lore items scattered on maps.
4. **XP/leveling system** — stat growth, skill unlocks.
5. **Status effects** — corruption, slow, stun, shield.
6. **Full enemy roster (Floors 1-4)** — all bestiary entries.
7. **Avery's full skill kit** — Crystal Prison, Crystal Sphere, Purify.
8. **Save system** — save points, localStorage, 3 slots.

---

### PHASE 2: Chapter 2 (Fran's Story)

11. **Fran's party** — Fran, Myst, Lynne stats/skills
12. **Bond Meter system** — Fran+Myst combo mechanics
13. **Floors 5-7 maps** — time-distortion themed layouts
14. **New enemies** — Phase Wraith, Temporal Shade, Echo Demon
15. **Chapter 2 story beats** — Fran's journey, Lynne's teaching

---

### PHASE 3: Chapter 3 + Boss (Convergence)

16. **Converged party** — Avery + Fran together
17. **Origin Crystal combo** — dual-Origin ultimate ability
18. **Floors 8-10 maps** — castle and Heart
19. **Boss: Deep Lord Fragment** — 3-phase battle
20. **Escape sequence** — post-boss transition

---

### PHASE 4: Epilogue & The Siege (Transition to Book 1 Ending)

21. **The Exit Scene:** Cinematic dialogue at the Vleyrra ruins. Lighting/visual shift to "The Siege" atmosphere.
22. **The Siege Mission:** 
   - Gameplay shift: High-intensity combat on a "destroyed" city outskirts map.
   - Objective: Escort civilians / Survive 10 turns.
   - Final Boss (Siege): Zachariel's Lieutenant.
23. **Ending Cinematic:** Retreat into Huge Woods.
24. **Polish:** Battle animations, skill effects, damage numbers, and audio.
25. **Full playthrough test** — all chapters, balance pass.

---

## File Structure

```
Games/
  labyrinth/
    index.html        ← complete game (single file)
    PLAN.md           ← this document
```

All code, art references, and data in one file — matching the existing Lore Navigator game pattern. Art assets referenced by filename; you'll create them separately and drop them into the same folder.

---

## Art Asset Wishlist (For Your Reference)

*You said you'll handle art separately — this is just a reference list so you know what the game needs:*

### Sprites (Canvas-rendered)
| Asset | Count | Notes |
|-------|-------|-------|
| Character sprites (walking, 4 directions) | 5 | Avery, Nate, Alyse, Fran, Callen |
| Character portraits (battle/dialogue) | 6 | Above + Myst, Lynne |
| Enemy sprites (battle + map) | 12 | All bestiary entries |
| Boss sprite (3 phases) | 1 | Deep Lord Fragment |
| Tile sprites (floor, wall, door, chest, etc.) | ~10 | 16x16 or 32x32 each |
| NPC sprites | 3 | Ghostly figure, Lynne, minor NPCs |
| Item icons | ~15 | Consumables, equipment, key items |
| Backgrounds (battle screens) | 5 | One per floor theme |
| Effects (crystal shard, light bolt, etc.) | ~10 | Per skill, can be simple particles |

### UI Elements
| Asset | Notes |
|-------|-------|
| Title screen background | Atmospheric labyrinth scene |
| Menu backgrounds | Can reuse battle backgrounds |
| Cursor/highlight sprites | For menu selection |
| Damage number font | Simple numeric font or bitmap |

---

## Design Decisions & Rationale

### Why Turn-Based (Not Action Combat)?
- Matches the Golden Sun / FF / Pokémon reference
- Allows strategic use of each character's unique kit
- Fits the lore: the Labyrinth is about discovery and problem-solving, not reflexes
- Easier to balance and playtest

### Why Three Chapters (Not One Long Run)?
- The lore has three characters whose stories converge — the game should reflect that
- Different party compositions keep combat fresh
- Fran's chapter plays mechanically different (Bond Meter, Origin focus)
- The Avery+Fran convergence is the lore highlight — making it a gameplay climax

### Why No Equipment/Weapon Grinding?
- Characters' power is innate (Origin, crystal, light magic) — not gear-dependent
- Keeps focus on ability usage and strategy
- Equipment is limited to Focus Items (stat tweaks, not core progression)
- Prevents the game from becoming a numbers slog

### Why Visible Encounters (Not Random)?
- Player agency: choose to engage or avoid
- Matches the exploration mood — the Labyrinth is atmospheric
- Stealth option adds light tactical layer before combat
- Modern RPG standard (post-2000s), fits the design vision

### Why Canvas (Not DOM)?
- Sprite rendering at 60fps needs Canvas
- Battle animations, damage numbers, particle effects all benefit
- Matches the technical decision you confirmed
- DOM would work for UI overlay (menus, dialogue) — hybrid approach

---

## Open Design Questions

- [ ] **Floor count:** 10 floors total enough, or should some chapters have more?
- [ ] **Difficulty curve:** Should there be an Easy/Normal/Hard mode?
- [ ] **New Game+:** After completing, carry over stats/equipment for replay?
- [ ] **Sound implementation:** Include audio from Phase 1, or defer to Phase 2?
- [ ] **Lynne's fate:** After Chapter 3, does Lynne exit with Avery/Fran, or stay behind? (Lore doesn't specify)
- [ ] **Callen's epilogue length:** Single battle, or should it be a short playable section (2-3 encounters)?
- [ ] **Accessibility:** Colorblind-friendly palette, text size options, turn speed controls?

---

## References

- **Lore source:** `../../Lore/DEEPBORN.md` (Underworld, Deepborn layers, seal mechanics)
- **Plot context:** `../../Lore/PLOT.md` (Book 1 Part 2, Ch. 63 intersection)
- **Game format:** `../GAME_FORMAT.md` (existing game patterns, save conventions)
- **Existing games:** `../mythanar/PLAN.md`, `../deepborn-siege/PLAN.md` (reference structures)
- **Idle game plan:** `../../Idle/PLAN.md` (HTML/JS architecture precedent)
