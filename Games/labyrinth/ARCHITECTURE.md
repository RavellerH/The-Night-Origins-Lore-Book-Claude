# Labyrinth — Architecture & Collaboration Plan

> **Last updated:** 14 April 2026
> **Phase:** Phase 0 complete → Phase 1 (Multi-floor, party expansion)

---

## Role Division

### Qwen (Game Developer)
**Owns:** Game mechanics, architecture, engine code, systems, implementation, bug fixes.

**Files/Sections:**
- `FLOORS` data object and schema (data structure design)
- `loadFloor(floorNum)` — reads floor data, initializes state
- `initFloor()` — renders map tiles, spawns enemies/chests/NPCs
- `addPartyMember(charData)` — safely adds characters to party
- `saveGame()` / `loadGame()` — localStorage persistence
- `exitFloor()` — cleanup, persist floor state, transition to next floor
- Battle system code (`playerAction`, `enemyTurn`, `calcDamage`, `nextTurn`)
- Veil Pressure, Synergy Combos, Positioning, Taint, XP systems
- Game loop and state machine transitions
- Input handling and menu systems
- Performance optimization and rendering

### Gemini (Artist)
**Owns:** Visual design, CSS styling, art prompts, visual effects, animations, UI aesthetics.

**Files/Sections:**
- All CSS in `index.html` — colors, typography, layout, animations
- `ART_PROMPTS.md` — image generation prompts for game art
- Visual effects: Veil Pressure bar styling, combo text effects, damage numbers
- Battle UI styling — skill menus, item menus, turn queue appearance
- HUD design — character bars, resource display, taint indicators
- Title screen, transition screen, result screen visual design
- Canvas visual polish — map tile art, character sprites, enemy art, particles
- Floor visual themes (wall colors, floor colors, ambient particles)
- Animation keyframes for buff effects, purify, shield, veil break

### Claude (Writer)
**Owns:** All narrative content, lore accuracy, dialogue, enemy lore, story beats.

**Files/Sections:**
- `STORY_BEATS` — all dialogue text, narrative triggers
- `ENEMY_TYPES` — enemy names, descriptions, weakness lore
- `STORY.md` — narrative arc, floor-by-floor story beats
- `BATTLE_MECHANICS.md` — battle design notes (reference doc, maintained by Claude)
- Skill descriptions and lore names
- NPC encounter dialogues and party-join story text
- Focus Item lore descriptions and flavor text
- Rune Fragment lore text and research notes
- Floor names, chapter names, level names
- Victory/defeat story text per floor
- Lore verification against `/Lore/` files
- `SHARED_LOG.md` narrative entries

---

## Coordination Points (All Agents — Confirm Before Editing)

| Section | Who Edits | Notes |
|---------|-----------|-------|
| `gameState` object shape | **Qwen** | Qwen defines, Claude adds lore fields, Gemini adds visual fields |
| `FLOORS` object | **Qwen + Gemini** | Qwen: data structure. Gemini: visual themes per floor. Claude: names/lore |
| `handleInteraction()` | **Qwen** | Gemini/Claude provide content (visual triggers, story triggers) |
| `playerAction()` | **Qwen** | Claude provides skill data, Gemini provides visual effects |
| `render()` / `drawBattle()` | **Qwen + Gemini** | Qwen: logic. Gemini: visual polish, particles, animations |
| `index.html` CSS | **Gemini** | All styling |
| `index.html` JS logic (systems) | **Qwen** | Battle code, floor loading, state machine, menus |
| `index.html` JS data (content) | **Claude** | STORY_BEATS, ENEMY_TYPES, skills, dialogue text |
| `index.html` Canvas art | **Gemini** | Tile art, sprites, particles, visual effects |

---

## Data-Driven Floor System

### Core Principle
**Separate DATA from LOGIC.** Floor definitions live in a single data object. Engine code reads from data and doesn't need to know "Floor 3" is special — it just follows the data.

### `FLOORS` Object Schema

```js
const FLOORS = {
  1: {
    name: "The Demon Corridors",
    number: 1,
    chapter: "The Descent",
    
    tiles: [...],            // 10x10 tile grid
    entryPos: { x, y },
    stairsPos: { x, y },
    
    chests: [                // chest positions and loot
      { pos: { x, y }, loot: { healingDrop: 2, crystalShard: 1 } },
    ],
    
    enemies: [               // enemy positions and patrol patterns
      { pos: { x, y }, type: 'imp', patrol: 'circular' },
    ],
    
    collectibles: [          // Rune Fragments, research items
      { pos: { x, y }, type: 'runeFragment', id: 'rune_1' },
    ],
    
    npcs: [                  // NPC encounters (Frank, Alyse, Ellen)
      { pos: { x, y }, id: 'frank', trigger: 'approach' },
    ],
    
    storyTriggers: {
      onEnter: 'floor1_entry',
      midFloor: 'floor1_mid',
      onExit: null,
    },
    enemiesRequired: 4,
    
    visuals: {
      wallColor: '#2a1a3a',
      floorColor: '#1a1020',
      ambientParticles: 'purple_embers',
    },
    
    startVeilPressure: 10,
  },
  
  // Floors 2-10 follow same structure
};
```

### `gameState` Object Shape

```js
let gameState = {
  currentFloor: 1,
  party: PARTY,              // mutable copy (HP, buffs, taint, level)
  inventory: { ... },
  gold: 0,
  xp: { avery: 0, nate: 0 },
  level: { avery: 1, nate: 1 },
  
  floorState: null,          // per-floor state (reset each floor)
  // floorState = {
  //   chestsOpened: [],
  //   enemiesDefeated: 0,
  //   collectiblesFound: [],
  //   npcsMet: [],
  //   storyFlagsTriggered: [],
  //   veilPressure: 10,
  // }
  
  storyFlags: {              // persistent across floors (never reset)
    metFrank: false,
    alysePurified: false,
    ellenFound: false,
    franMet: false,
    comboUnlocked: false,
  },
  
  research: {                // collected fragments
    runeFragments: [],
    timeFragments: [],
    memoryFragments: [],
  },
  
  equipment: {               // Focus Items equipped
    nate: null,
    avery: null,
    fran: null,
  },
};
```

---

## Migration Plan (Don't Break What Works)

### Stage 1 — Wrap Floor 1 (No Behavior Change)
- Create `FLOORS` object with Floor 1 data from existing constants
- Keep `FLOOR_MAP`, `MAP_ENEMIES`, `CHEST_POS`, `STAIRS_POS` as references
- No behavioral changes — just restructure

### Stage 2 — Replace Hardcoded References
- Replace `FLOOR_MAP` → `FLOORS[gameState.currentFloor].tiles`
- Replace `CHEST_POS` → `FLOORS[floor].chests`
- Replace `MAP_ENEMIES` → `FLOORS[floor].enemies`
- Replace `STAIRS_POS` → `FLOORS[floor].stairsPos`

### Stage 3 — Add Floor 2 (Frank Encounter)
- Prove the system works with a second floor
- Test NPC encounter, party join logic, new enemy types

### Stage 4+ — Add Remaining Floors
- One floor at a time, each with unique content
- Validate save/load between floors

---

## Implementation Order

### Qwen (Foundation — Builds First):
1. `FLOORS` data schema and Floor 1 migration
2. `loadFloor(floorNum)` function
3. `initFloor()` function
4. `gameState` object and initialization
5. `saveGame()` / `loadGame()` (localStorage)
6. Party expansion (`addPartyMember`)
7. Level-up system with stat growth
8. Enemy scaling formulas per floor

### Gemini (Visuals — Builds in Parallel):
1. Visual polish for existing game elements (title screen, HUD, menus)
2. Veil Pressure gauge styling and color transitions
3. Combo text effects (Prismatic Burst, Veil Shatter animations)
4. Damage number styling improvements
5. Floor visual themes (colors, particles, ambient effects)
6. Character/enemy sprite art improvements on canvas
7. Animation keyframes for buff/purify/shield effects
8. `ART_PROMPTS.md` updates for new art needs

### Claude (Content — Builds in Parallel):
1. Expand `ENEMY_TYPES` for Floors 1-4 (names, weaknesses, lore descriptions)
2. Expand `STORY_BEATS` for Frank, Alyse, Ellen encounters
3. Focus Item lore (Light Pendant, Crystal Lens, Origin Gem descriptions)
4. Rune Fragment lore text and research notes
5. NPC encounter dialogues and party-join story text
6. Victory/defeat story text per floor (Floors 1-10)
7. Floor names and chapter names from `LEVEL_PLAN.md`
8. Lore verification — all content matches `/Lore/` files

---

## Reference Documents

| Document | Purpose | Owner |
|----------|---------|-------|
| `PLAN.md` | Original development plan | Shared |
| `COLLAB_PLAN.md` | Task division between Gemini + Claude | Shared |
| `STORY.md` | Narrative arc, floor-by-floor story | Claude |
| `BATTLE_MECHANICS.md` | Battle design specs, formulas, skills | Claude (maintained) |
| `LEVEL_PLAN.md` | 10-floor layout + epilogue | Shared |
| `ARCHITECTURE.md` | This file — system architecture | Shared |

---

## Lore Source Files

All game content must be consistent with the lore files in `../../Lore/`:

- `CHARACTERS.md` — Character backgrounds, abilities, relationships
- `MAGIC_SYSTEM.md` — Magic rules (Light burns, Dark heals, Crystal purifies)
- `PLOT.md` — Story events, timeline, character arcs
- `TIMELINE.md` — Chronological events, what happens when
- `FACTIONS.md` — Faction relationships, enemy types
- `KINGDOMS_AND_LOCATIONS.md` — Geography, location descriptions

**Rule:** If game content conflicts with lore, lore wins. Update the game, not the lore.

---

## Communication Protocol

- **Before editing shared sections:** Check this file, confirm role, note what you're changing
- **After major changes:** Update this file if the data shape or function signatures change
- **Lore questions:** Claude verifies against `/Lore/` files, Gemini implements the result
- **Bug fixes:** Whoever owns the section fixes it
