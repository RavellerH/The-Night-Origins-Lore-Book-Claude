# Labyrinth — Shared Dev Log

> Both agents write entries here. Timestamps required. This is the single source of truth for what's being worked on and what's been completed.

---

## Format

```
[YYYY-MM-DD HH:MM] [AGENT] [STATUS] Description of change
```

**Status codes:**
- `DONE` — completed and working
- `WIP` — in progress, not yet complete
- `BLOCKED` — waiting on something else
- `REVERT` — change was reverted
- `NOTE` — informational, no code change

---

## Log

[2026-04-14 20:30] [QWEN] DONE Created ARCHITECTURE.md — 3-agent role division (Gemini/Claude/Qwen), data structures, migration plan, coordination points.

[2026-04-14 20:30] [QWEN] DONE Created SHARED_LOG.md for cross-agent monitoring.

[2026-04-14 20:30] [QWEN] DONE Battle mechanics: Veil Pressure, Synergy Combos, Positioning, Taint system, XP tracking.

[2026-04-14 20:35] [GEMINI] WIP Visual polish: HUD enhanced with Taint/Level indicators, Veil Pressure gauge with animated gradients, and cinematic Cutscene overlay.

[2026-04-14 20:40] [GEMINI] DONE Enhanced Battle VFX: Particle system implemented for magic trails, cast gathering effects, and hit bursts. Enemies now use projectiles for magic-based skills.

[2026-04-14 20:45] [GEMINI] DONE Floor 2 Implementation: "The Echo Chamber" layout, Shadow Echo enemies, and Frank NPC encounter (joins party on Floor 2). Improved floor transition system.

[2026-04-14 20:35] [GEMINI] NOTE Synchronized gemini_plan.md with ARCHITECTURE.md role division. Handed off system logic to Qwen.

[2026-04-14 20:30] [QWEN] DONE Fixed: buff/shield/debuff/purify skills freezing (missing `nextTurn()` call).

[2026-04-14 20:30] [QWEN] DONE Fixed: Enemy `buffs` undefined, `showSkillMenu` crash guard.

[2026-04-14 21:00] [QWEN] DONE FLOORS data schema — created `FLOORS` object with Floor 1, backwards-compatible aliases.

[2026-04-14 21:00] [QWEN] DONE `gameState` object — unified state with currentFloor, xp, level, gold, inventory, floorState, storyFlags, research, equipment.

[2026-04-14 21:00] [QWEN] DONE `loadFloor(floorNum)` — reads FLOORS data, resets floorState, positions player, spawns enemies.

[2026-04-14 21:00] [QWEN] DONE `saveGame()` / `loadGame()` — updated to use gameState, saves storyFlags/research/equipment/taint/position.

[2026-04-14 21:00] [QWEN] DONE `startGame()`, `resetGame()`, `initGame()` — updated to use new floor loading system.

[2026-04-14 20:46] [CLAUDE] DONE Canon audit Floors 2-4 — verified Alyse alive at Ch.63, Frank magic constraint, Ellen herbalist role, Alyse no-heal rule, Twin Aura unnamed until Floor 8.
[2026-04-14 20:46] [CLAUDE] DONE Character skill specs — Frank Dewflame (gadget/machine) + Ellen Shadowseer (item-efficiency/ward) written to BATTLE_MECHANICS.md.
[2026-04-14 20:46] [CLAUDE] DONE Crystal Purify lock — `unlocked: false` on skill, `showSkillMenu` filter, `unlockCrystalPurify()` function, documented in BATTLE_MECHANICS.md → MECHANIC LOCKS.
[2026-04-14 20:46] [CLAUDE] DONE Dialogue beats Floors 2-4 — JS-ready STORY_BEATS entries + trigger chain order written to STORY.md.
[2026-04-14 20:46] [CLAUDE] DONE Cutscene system — 5 cutscenes (portal_entry, frank, alyse, ellen, convergence) with lore-canon choices → mechanical effects. NOTE: CSS and JS engine code included — Gemini should review/restyle CSS, Qwen should review engine logic.
[2026-04-14 20:46] [CLAUDE] DONE Session log written to claude_plan.md.
[2026-04-14 20:46] [CLAUDE] NOTE Canon flag: LEVEL_PLAN.md Epilogue lists "Lightning Mages" as Zachariel's forces — violates canon (Kingdom of Lightning destroyed before story). Needs correction before Epilogue content is written.
[2026-04-14 20:46] [CLAUDE] NOTE Unverified line in LEVEL_PLAN.md Floor 8: "Then you are my past!" — no script source found. Correct script: Lynne says "she has the same aura as you, Fran… she is the Origin" + dual "What?!". Needs author confirmation.
[2026-04-14 21:10] [CLAUDE] DONE ENEMY_TYPES expanded — 5 new enemies added to index.html: shadowEcho (Floor 2), mindlessDrone (Floor 3), corruptedWolf (Floor 3), taintedMage (Floor 4, elite), voidSentinel (Floor 4, elite). All lore-verified. New status effects (taint, taintAll, bleed, atkDown, veilBoost, stun) documented — Qwen to implement handlers.
[2026-04-14 21:10] [CLAUDE] DONE Focus Item specs written to BATTLE_MECHANICS.md — Light Pendant (Nate/Floor 3), Crystal Lens (Avery/Floor 4), Origin Gem (Fran/Floor 7), Purifying Drop (consumable/Floor 3). Includes lore, flavor text, mechanical effects.
[2026-04-14 21:10] [CLAUDE] DONE Rune Fragment texts written to STORY.md — 6 fragments total (3 for Avery Ch.1, 3 for Fran Ch.2). All lore-verified against DEEPBORN.md. Fragment 6 (Origin Fragment) is the narrative pivot that foreshadows the convergence.

[2026-04-14 22:00] [CLAUDE] NOTE Qwen unavailable. Claude taking over engine role for this session.

[2026-04-14 22:00] [CLAUDE] DONE Bug fix: `buildBattleHUD()` was called in `initGame()` but not defined — added alias. Game was crashing on start.
[2026-04-14 22:00] [CLAUDE] DONE Bug fix: `floorNumber` variable never updated by `loadFloor()` — veil pressure / crushing damage always used Floor 1 math. Now synced in `loadFloor()`.
[2026-04-14 22:00] [CLAUDE] DONE Bug fix: XP tracked in `xp` var but saved from `gameState.xp` — desync after any battle. Now `endBattleVictory` writes to `gameState.xp` and syncs alias.
[2026-04-14 22:00] [CLAUDE] DONE Enemy status effect handlers — `applyEnemyStatus()` function added. All enemy `skill.status` values now fire: taint (+1 to target), taintAll (+1 all party), bleed (5HP/turn 2t), atkDown (-25% 2t), defDown (-30% 2t, Feral Howl = party AoE), veilBoost (+10% veil), stun (1t skip), slow (-50% spd 2t).
[2026-04-14 22:00] [CLAUDE] DONE Bleed DoT — `nextTurn()` now processes bleed buffs at the start of each turn before veil check.
[2026-04-14 22:00] [CLAUDE] DONE Level-up system — `checkLevelUp()` function added with stat growth tables (Avery/Nate/Frank/Ellen). Called after `endBattleVictory` reward display. Shows "★ LEVEL UP!" text on result screen.
[2026-04-14 22:00] [CLAUDE] DONE Enemy stat scaling — `createEnemy()` now scales HP/ATK/DEF/MAG by +15% per floor above 1 (capped at +90%). Speed not scaled to preserve turn-order feel.
[2026-04-14 22:00] [CLAUDE] DONE Enemy skill selection — was only picking skills[0] or skills[1]; skills[2] never used. Now: 50% main attack, 50% random from remaining skills.

[2026-04-14 22:30] [CLAUDE] DONE 3-wide battle layout — Alyse Lucent added as ALYSE_TEMPLATE (Light Support, lore-accurate: no heal, light butterflies, SPD 10 / MAG 14). Skills: Light Butterfly (1.0 dmg), Butterfly Shield (25% absorb), Wing Flash (1.4 dmg).
[2026-04-14 22:30] [CLAUDE] DONE partyRenderPos updated to y=[210,330,450] — 3 chars spaced 120px, all clear of battle UI overlay (starts at canvas y=518). Name labels at max y=488.
[2026-04-14 22:30] [CLAUDE] DONE Party renderer: hair/eye colors now stored per-character (hairColor/eyeColor) instead of index ternary. Avery/Nate colors unchanged. Alyse: pale blonde hair, near-white eyes.
[2026-04-14 22:30] [CLAUDE] DONE Bug fix: animateMeleeAttack() — enemies were dashing RIGHT (same direction as party). Now uses dashDir=-1 for enemies, +1 for party. Also fixes target recoil direction.
[2026-04-14 22:30] [CLAUDE] DONE animateFlee() and endBattleDefeat() now iterate PARTY array dynamically — no longer hardcodes PARTY[0]/PARTY[1] only.
[2026-04-14 22:30] [CLAUDE] DONE addAlyseToParty() function — production: call from alyse_names_it callback on Floor 3. Prototype: Tab key during explore adds her instantly.
[2026-04-14 22:30] [CLAUDE] DONE HUD flex layout tightened (min-width 190px, gap 10px) to fit 3 character bars in the 1366px header.

[2026-04-14 23:00] [CLAUDE] DONE Formation screen — new `formation` state added between explore collision and battle. startBattle() now calls enterFormation() which shows the formation screen; confirmFormation() does the actual battle setup.
[2026-04-14 23:00] [CLAUDE] DONE Formation drag — mousedown on char sprite (radius 36 hit test), mousemove updates ghost, mouseup drops into front zone (x < 360) or back zone (x >= 360). Cursor changes to grab/grabbing. Drag cancelled on mouseleave.
[2026-04-14 23:00] [CLAUDE] DONE Formation renderer — left column FRONT (+15% ATK, +25% DMG), right column BACK (-25% DMG), dashed divider, drop zone highlight during drag, enemy preview (dimmed) on right, ENTER to confirm.

---

## Active Work (What Each Agent Is Doing Right Now)

| Agent | Role | Working On | Status | Blocking |
|-------|------|-----------|--------|----------|
| QWEN | Game Dev | Multi-floor engine (loadFloor, FLOORS schema, gameState) | NEXT | — |
| GEMINI | Artist | Visual polish, cutscene CSS review | NEXT | — |
| CLAUDE | Writer | ENEMY_TYPES expansion (Floors 2-4), Focus Item lore, Rune Fragments | NEXT | — |

---

## Qwen's Queue (Game Dev — What Qwen Should Work On Next)

1. `FLOORS` data schema — migrate Floor 1 constants into structured object
2. `loadFloor(floorNum)` — read from `FLOORS`, initialize `floorState`
3. `gameState` object — replace scattered variables with unified state
4. `saveGame()` / `loadGame()` — localStorage persistence
5. `addPartyMember()` — support 3+ characters in party
6. Level-up system implementation (stat growth, level-up animation)
7. Enemy scaling formulas per floor
8. 3-wide battle layout support (for when Alyse joins)

---

## Gemini's Queue (Artist — What Gemini Should Work On Next)

1. Visual polish — title screen, HUD, menus (CSS improvements)
2. Veil Pressure gauge — styling improvements, color transitions
3. Combo text effects — Prismatic Burst gold glow, Veil Shatter purple flash
4. Damage number styling — better fonts, sizes, critical hit emphasis
5. Floor visual themes — different colors/particles per floor
6. Character/enemy canvas art — improve sprites
7. Animation keyframes — buff/purify/shield CSS effects
8. Update `ART_PROMPTS.md` for new art needs

---

## Claude's Queue (Writer — What Claude Should Work On Next)

1. Expand `ENEMY_TYPES` for Floors 1-4 (Shadow Echo, Mindless Drone, Tainted Mage, Corrupted Wolf, Void Sentinel)
2. Expand `STORY_BEATS` for Frank, Alyse, Ellen encounters
3. Focus Item lore (Light Pendant, Crystal Lens, Origin Gem descriptions)
4. Rune Fragment lore text and research notes
5. NPC encounter dialogues and party-join story text
6. Victory/defeat story text per floor (Floors 1-10)
7. Floor names and chapter names from `LEVEL_PLAN.md`
8. Lore verification — all content matches `/Lore/` files

---

## Resolved Issues

| Date | Issue | Resolution |
|------|-------|------------|
| 2026-04-14 | STORY.md said Alyse had "Deepborn corruption" | Fixed — changed to "ambient veil taint" (lore-accurate) |
| 2026-04-14 | Nate had "Radiant Heal" skill | Fixed — replaced with "Radiant Shield" (Light magic can't heal) |
| 2026-04-14 | Battle buff/shield/debuff/purify skills froze | Fixed — added `animating = false` + `nextTurn()` |
| 2026-04-14 | Enemy `buffs` undefined on debuff | Fixed — added `buffs: []` to `createEnemy()` |
| 2026-04-14 | `showSkillMenu` crashed on undefined char | Fixed — added null/HP guard |

---

## Open Questions (Need Discussion)

| Question | Asked By | Status |
|----------|----------|--------|
| Floor map size: stay 10x10 or expand to 14x14 for deeper floors? | — | UNDECIDED |
| Should party members be controllable in explore or just Avery? | — | UNDECIDED |
| How does Floor 5 POV switch (Avery → Fran) work mechanically? | — | UNDECIDED |
| Should Veil Pressure reset between floors or persist? | — | UNDECIDED |
