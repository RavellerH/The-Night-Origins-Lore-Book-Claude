# Labyrinth — Collaborative Development Plan (Gemini & Qwen)

This document outlines the shared roadmap for the Labyrinth RPG. Gemini and Qwen will use this to track progress and divide tasks.

## Phase 0: Prototype Polish (v0.1.x)
**Goal:** Ensure Floor 1 is a "perfect" vertical slice.

- [x] **Dynamic HUD:** Refactor HUD code to support 1-3 party members dynamically (Gemini, 2024-04-14).
- [x] **Save System:** Implement `localStorage` saving (Gemini, 2024-04-14).
    - [x] `saveGame()`: Triggered at stairs or auto-save.
    - [x] `loadGame()`: Title screen "Continue" button logic.
    - [x] Data shape includes: party stats, gold, inventory, current floor, defeated enemies.
- [x] **Combat Polish:**
    - [x] Ensure "Super Effective" and "Critical" text is highly visible (Gemini, 2024-04-14).
    - [x] Add subtle "Turn Start" indicator (pulsing ring) for the active character (Gemini, 2024-04-14).
- [ ] **Inventory System:** Add an "Inventory" button in exploration mode (Key: `I`).

## Phase 1: Descent (Chapter 1 Complete)
**Goal:** Full Chapter 1 experience (Floors 1-4) with Avery's party.

- [ ] **Multi-Floor Architecture:**
    - [ ] Move map data to a `MAPS` object indexed by floor number.
    - [ ] Implement `loadFloor(number)` function to reset enemies, player position, and tiles.
- [ ] **Experience & Leveling:**
    - [ ] Add `xp` and `level` to character objects.
    - [ ] Victory screen: Show XP gained and level-up popups.
    - [ ] Stat growth formulas (see `PLAN.md`).
- [ ] **Narrative & Story (Enriched):**
    - [ ] Implement **Floor 2 Dialogue:** Frank's temporal disorientation.
    - [ ] Implement **Floor 3 Event:** Alyse found weakened from veil exposure; Avery's "Purify" awakens to cleanse ambient taint.
    - [ ] Implement **Floor 4 Dialogue:** Ellen's "week-long" trauma.
- [ ] **Rune Fragment System:**
    - [ ] Add hidden "Rune Fragment" tiles that trigger lore dialogue.
- [ ] **Party Expansion:**
    - [ ] **Alyse (Light Support):** Joins at Floor 3.
    - [ ] Update Battle Engine to handle 3-wide party layout.
- [ ] **Bestiary (Floors 2-4):**
    - [ ] Floor 2: More Imps and Creepers.
    - [ ] Floor 3: Mindless Drone (Corruption mechanics).
    - [ ] Floor 4: Corrupted Wolf / Tainted Mage.
- [ ] **Equipment (Focus Items):**
    - [ ] Add simple equipment slots (Focus, Charm).
    - [ ] Items found in chests or as rare drops.

## Technical Tasks (Gemini Priority)
1. **Refactor HUD/Battle UI:** Make them data-driven so adding characters doesn't require CSS/HTML changes.
2. **Save/Load Logic:** Handle state persistence.
3. **Map Design (Floor 2):** Create the tile layout for the next area.

## Creative Tasks (Claude Priority)
1. **Dialogue & Story:** ~~Expand the `STORY_BEATS` for Floor 2-4.~~ **[DONE 2026-04-14 — Claude]** Beats written to `STORY.md` with JS-ready format and trigger notes.
2. **Character Skill Specs (Frank + Ellen):** ~~Pending.~~ **[DONE 2026-04-14 — Claude]** Specs written to `BATTLE_MECHANICS.md`. Frank = gadget/machine/light-fuel. Ellen = item-efficiency/ward/tonic.
3. **Mechanic Lock (Crystal Purify):** ~~Pending.~~ **[DONE 2026-04-14 — Claude]** Lock documented in `BATTLE_MECHANICS.md → MECHANIC LOCKS`. Implementation (flag in skill object + menu filter) is Gemini's task.
4. **Canon Audit Floors 2-4:** ~~Pending.~~ **[DONE 2026-04-14 — Claude]** Alyse confirmed alive at Ch. 63 ✓. Frank's weak-magic constraint applied ✓. Alyse's no-heal constraint applied ✓. Twin Aura unnamed until Floor 8 ✓.
5. **Animation Polish:** Refine the CSS/Canvas animations for skills.
6. **Enemy AI:** Add more variety to enemy skill selection logic.

---

## Change Log
- **2024-04-14:** Gemini created this plan to sync with Qwen code and current `index.html` state.
