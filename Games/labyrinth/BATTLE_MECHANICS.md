# Battle Mechanics — Design Notes

> **Last updated:** 14 April 2026
> **Status:** Phase 0 → Phase 1 transition
> **Verified against:** Lore files (MAGIC_SYSTEM.md, CHARACTERS.md, TIMELINE.md, PLOT.md)

---

## LORE-VERIFIED MAGIC RULES (DO NOT VIOLATE)

### Dark Magic = Healing
- **Shadow/Void magic is the ONLY healing magic in Kalmirth**
- Light magic BURNS — it is offensive, heat-based
- Dark magic SUSTAINS people — Descendants can close wounds, stabilize the dying
- At Origin-level, a Dark Mage could resurrect the dead as undead (no living dark mage has this power)
- **No Dark Mage is in the current party** — healing comes from items only at this stage

### Light Magic = Offensive/Support
- Creates and projects intense light (blinding, weapons, platforms)
- Forms light swords, light bows, light arrows
- Can create light butterflies, light platforms (Light Walk)
- Can coat/charge magical objects (Ravinger's ball of light)
- **CANNOT HEAL** — this is a hard rule

### Crystal Magic (Avery) = Replicate + Purify
- Can replicate ANY elemental magic by forming a crystal of that element
- Can encase beings in crystal (time nearly stopped, heartbeat maintained)
- Can REVERSE encasement (discovered at Oath Circle)
- Can PURIFY/restore — crystal magic can cleanse taint and ambient corruption
- Crystal magic is NOT full healing — it restores small amounts while purifying

### Natural Healing = Items Only
- Daun Laeiorra (Blue Medicinal Leaves) = natural healing resource
- Healing Drop / Greater Healing Drop = crafted from these leaves
- **Items are the primary healing method** for the party

---

## IMPLEMENTED SKILLS (Lore-Accurate)

### Avery (Crystal Origin)

| Skill | Type | Effect | MP Cost | Lore Basis |
|-------|------|--------|---------|------------|
| Crystal Shard | Attack | Single-target crystal damage (power 1.2) | 8 | Crystal projectile |
| Crystal Barrier | Buff | Ally DEF +50% for 3 turns | 10 | Crystal walls block attacks |
| Crystal Purify | Restore | Removes 1-2 status effects, restores 15 HP | 12 | Crystal cleanses/restores |
| Crystal Prison | Attack+Debuff | Damage (power 1.5) + Slow (50% SPD for 2 turns) | 15 | Partial encasement |
| Crystal Sphere | Ultimate | AoE damage to all enemies (power 1.8) | 25 | Giant crystal sphere drain |

**Key change:** Purify is NOT a heal — it's a cleanse with minor HP restore. It removes status effects (taint, slow, stun, silence, corruption) and restores 15 HP as a side effect.

### Nate (Light Magic via Shield Artifact)

| Skill | Type | Effect | MP Cost | Lore Basis |
|-------|------|--------|---------|------------|
| Light Bolt | Attack | Single-target light damage (power 1.3) | 8 | Light arrow/projectile |
| Blinding Flash | Debuff | Lowers enemy accuracy for 2 turns | 10 | Light magic blinds |
| Light Walk | Self-Buff | Gives Nate +1 extra turn in next round (speed +30%) | 8 | His signature technique |
| Aurora Wave | AoE | Light damage to all enemies (power 1.5) | 18 | Large-scale light projection |
| Radiant Shield | Buff | Creates barrier absorbing 20% incoming damage for 3 turns | 12 | Light coats/protects objects |

**Key change:** Radiant Heal replaced with Radiant Shield. Nate has ZERO healing skills. All his abilities are offensive or support.

### Frank Dewflame (Light Magic — Tinkerer / Machine Combatant)

**Canon basis:** Frank's light magic is weak by mage standards. He does not fight with it directly — he uses it as fuel for machines. His gun shoots concentrated light (confirmed script: "stronger than hand-cast magic"). His skills are gadget/machine based; his light magic is the engine, not the weapon.

| Skill | Type | Effect | MP Cost | Lore Basis |
|-------|------|--------|---------|------------|
| Light Gun | Attack | Single-target light damage (power 1.4) — machine-amplified | 6 | Homemade gun: concentrated light, stronger than hand-cast |
| Deploy Trap | Delayed | Next enemy to act takes bonus damage (power 1.2), then trap depletes | 8 | Deployable magical traps |
| Light Fuel | Buff | One ally ATK +30% for 2 turns | 10 | Light magic as power source, channeled into ally |
| Gadget Shield | Buff | Party physical DEF +20% for 2 turns | 12 | Machine construct deployed as barrier |

**Veil note:** Frank's Light Gun reduces veil pressure by only -5% (vs. Nate's -12%). His light is weak and machine-routed — not the clean Light magic that pushes back the veil.

**No healing.** No standard mage attacks. All skills are mechanical amplifications of weak light magic.

### Ellen Shadowseer (Light Magic — Herbalist / Defensive Support)

**Canon basis:** Ellen is primarily an herbalist. Her light magic is weak. Her combat role is item efficiency + protective buffs. Per lore: light magic can coat/charge objects — Ellen uses this to enhance her herbs. Her skills are support and defensive; she has zero offensive output.

| Skill | Type | Effect | MP Cost | Lore Basis |
|-------|------|--------|---------|------------|
| Infuse Herb | Restore | Uses one Healing Drop from inventory with +60% effect | 5 | Light-infused herbs — light coats/charges medicinal objects |
| Light Ward | Buff | Party -15% damage taken for 2 turns | 8 | Weak light as protective layer |
| Steady Hands | Buff | One ally: next skill guaranteed accuracy (cannot miss) | 6 | Herbalist precision, steadying influence |
| Tonic | Restore | Removes 2 Taint from one ally, no item consumed | 10 | Herbalist compound — cleanses ambient corruption |

**Tonic vs. Crystal Purify distinction:**
- Crystal Purify: removes 1-2 status effects + 3 Taint + 15 HP restore. Full cleanse.
- Tonic: removes 2 Taint only. No status removal. No HP. Herbalist method, not magic origin.

**No healing spells.** Infuse Herb requires an item in inventory — it's a skill that improves item use, not a spell.

---

## MECHANIC LOCKS

### Crystal Purify — Floor 3 Lock
Crystal Purify is available in Avery's data from the start but **must be hidden from her action menu** until the Floor 3 Alyse scene completes (`alyse_names_it` beat fires and callback resolves).

- Before lock releases: skill exists in `avery.skills` array but filtered out of the menu render
- After lock releases: appears in skill menu normally
- Narrative reason: Avery fears her crystal is too close to the Labyrinth's dark energy. The Alyse scene is the moment she learns it restores instead of destroys.
- Implementation: add `unlocked: false` flag to the skill object; `showSkillMenu()` filters `skill.unlocked !== false`

---

## NEW MECHANICS (Phase 1+)

### 1. Veil Pressure (Environmental Mechanic)

**Concept:** The Labyrinth is a Deepborn weak veil — it gets heavier as you descend. Each turn that passes, veil pressure increases.

**Implementation:**
- Each battle has a `veilPressure` gauge (0-100%)
- Starting pressure = `10 + (floorNumber - 1) * 8` (Floor 1 = 10%, Floor 2 = 18%, etc.)
- Every turn: `veilPressure += 5 + floorNumber * 0.5`
- Thresholds:
  - **50%:** All Deepborn enemies gain +20% ATK (veil empowers them)
  - **80%:** Party takes `3 + floorNumber * 2` damage per turn (veil crushing)
  - **100%:** "Veil Break" — strongest living enemy gets a FREE attack immediately
- Using Origin/Crystal/Light skills **reduces** veil pressure:
  - Crystal skills: `-10%`
  - Light skills: `-12%`
  - Nature/Dark/Fire skills: `+3%` (these magic types feed the veil)

**Why:** Creates urgency. Makes Light/Crystal skills doubly valuable (damage + veil reduction). Scales with floors naturally.

### 2. Synergy Combos (Party Cohesion)

**Concept:** When party members use complementary skills in sequence, they trigger combo effects.

**Implementation:**
- Track `lastSkillType` and `lastTargetIdx` per character
- After a character acts, check if the NEXT character's action matches a combo pattern:

| Combo Pattern | Trigger Condition | Effect |
|---------------|-------------------|--------|
| Prismatic Burst | Crystal skill → Light skill on same target | +25% bonus damage, ignores DEF |
| Refracted Strike | Barrier on self → Attack next turn | +30% damage (barrier energy channels through) |
| Radiant Recovery | Heal/Purify on ally → Heal/Purify on same target within 2 turns | Target gains +20% MAX HP for rest of battle |
| Veil Shatter | Two Light/Crystal skills on same enemy within 3 turns | -15% veil pressure, enemy stunned 1 turn |

**UI:** Show combo text in battle log: "PRISMATIC BURST!" with gold text.

### 3. Positioning (Front/Back Row)

**Concept:** Characters and enemies have positions that affect combat.

**Implementation:**
- Each character has `position: 'front' | 'back'` (default: front)
- **Front row:** +15% ATK dealt, +25% damage taken
- **Back row:** -25% damage taken, melee attacks cannot target enemies from back
- The "Defend" action includes a position swap option (press Defend again while defending to swap)
- Enemy types have starting positions:
  - Scouts: front row (melee)
  - Casters: back row (ranged)
  - Brutes: front row (tank)

**UI:** Show row position in battle HUD. Small indicator next to character sprite.

### 4. Taint Accumulation (Long-term Mechanic)

**Concept:** Being in the Labyrinth leaves marks. Each battle accumulates "Taint" on party members.

**Implementation:**
- Each battle ends with `+1 + Math.floor(battleLength / 3)` Taint per character (min 1, max 4)
- Taint effects:
  - **1-4:** No effect
  - **5-7:** -10% healing received from items
  - **8-9:** -20% healing received, random 5 HP loss per 10 exploration steps
  - **10+:** "Veil Sickness" — random 10 HP loss per 10 steps, -15% all stats
- **Crystal Purify** removes 3 Taint from target character
- **Clean Zone** (safe room) removes 2 Taint from whole party
- **Exiting a floor** removes ALL Taint (safe between floors)

**UI:** Show Taint count in HUD. Warning indicator at 5+, dangerous at 8+.

### 5. Level-Up System (Stat Growth)

**Concept:** Characters level up based on XP earned from battles.

**Implementation:**
- **XP per battle:** `sum of (enemy.level * 10 + enemy.goldDrop average)`
- **XP curve:** `level * 100 XP` to reach next level
- **Stats per level:**

| Stat | Avery Growth | Nate Growth |
|------|-------------|-------------|
| HP | +15 | +12 |
| MP | +10 | +8 |
| ATK | +2 | +3 |
| MAG | +4 | +2 |
| DEF | +2 | +3 |
| SPD | +1 | +2 |
| LUK | +1 | +1 |

- **New skills unlock at levels:**
  - Avery: Lv3 → Crystal Barrier, Lv5 → Crystal Purify, Lv7 → Crystal Prison, Lv10 → Crystal Sphere
  - Nate: Lv3 → Blinding Flash, Lv5 → Light Walk, Lv7 → Aurora Wave, Lv10 → Radiant Shield

**UI:** Level-up animation with stat growth display. Player chooses +1 bonus stat on level up.

### 6. Enemy Scaling with Floors

**Concept:** Enemies grow stronger on deeper floors.

**Implementation:**
- Each enemy type has `baseStats` and `scalingPerFloor`
- On floor N: `enemy.stat = baseStat + scalingPerFloor * (N - 1)`
- Floor 1 enemies: base stats
- Floor 2: +15% all stats, new enemy types unlocked
- Floor 3: +30% all stats, elites appear
- Floor 4: +50% all stats, mini-bosses appear
- Floor 5+: +15% per floor, bosses every 3 floors

**Formula example (Deepborn Scout on Floor 3):**
- HP: `45 + 10 * (3-1) = 65`
- ATK: `8 + 1.5 * (3-1) = 11`
- DEF: `3 + 0.5 * (3-1) = 4`
- Gold drop: same base (gold is economy, not combat balance)

---

## HEALING METHODS SUMMARY

| Method | Amount | When Available | Notes |
|--------|--------|---------------|-------|
| Healing Drop (item) | +30 HP | From start | Crafted from Daun Laeiorra |
| Greater Healing Drop (item) | +80 HP | From Floor 2+ | Stronger concentration |
| Blue Leaf Poultice (item) | +15 HP/turn for 3 turns | Rare find | HoT effect |
| Crystal Purify (skill) | +15 HP + cleanse taint | Avery Lv5+ | Not a heal — purification |
| Inn (rest between floors) | Full HP/MP restore | After completing a floor | Safe zone |

**No character has a true healing skill.** All healing is item-based or environmental.

---

## ENEMY STATUS EFFECTS (New — Qwen to implement handlers)

| Status | Applied By | Effect | Duration |
|--------|-----------|--------|----------|
| `taint` | Corrupt Strike, Corruption Lunge | Target.taint += 1 | Instant |
| `taintAll` | Taint Blast | All party members taint += 1 | Instant |
| `bleed` | Savage Bite | 5 HP damage/turn | 2 turns |
| `atkDown` | Whisper | Target ATK -25% | 2 turns |
| `veilBoost` | Veil Pulse, Veil Wall | battleState.veilPressure += 10 | Instant |
| `stun` | Seal Strike | Target skips next turn | 1 turn |

`slow` and `defDown` already implemented. `lifesteal` (Devour) and AoE (Crushing Force) noted for Qwen — simplified to single-target until implemented.

---

## FOCUS ITEMS (Equipment System — Phase 1+)

Focus Items equip in the **Focus slot** (one per character). Charms equip in the **Charm slot**. Each character has one of each. Items are found in chests or as rare drops on specific floors.

### Light Pendant *(Charm — Nate)*
- **Found:** Floor 3 chest (Corrupted Grove)
- **Effect:** Light skills reduce Veil Pressure by an additional -5% (Nate's light skills now reduce -17% total). Nate SPD +1.
- **Lore:** An elven ornament from the Vleyrra Forest ruins — made by the elves who guarded the deep places. The light inside it doesn't know it's been lost down here for centuries.
- **Flavor text:** *"Made by the elves who guarded the deep places. The light inside it doesn't know it's lost."*

### Crystal Lens *(Focus — Avery)*
- **Found:** Floor 4 chest (Hall of Silence)
- **Effect:** All crystal skills power +0.2. Crystal Purify removes one additional status effect per use.
- **Lore:** A raw crystal fragment shaped into a lens by a previous explorer — someone who understood what crystal magic was for. Left in the Hall of Silence. They didn't make it out.
- **Flavor text:** *"Whoever made this knew what crystal magic was for. They just ran out of time to use it."*

### Origin Gem *(Focus — Fran)*
- **Found:** Floor 7 chest (Void Bridge)
- **Effect:** All of Fran's skills deal +15% damage to Deepborn-type enemies. Bond Sense skill range +50%.
- **Lore:** A fragment of raw seal energy — the actual material the Gods used when they contained the Deepborn. It resonates specifically with Origin magic. The Labyrinth didn't create it; it was here before the Labyrinth.
- **Flavor text:** *"It remembers the war that ended the gods' waking lives. It remembers them winning. It does not know they never woke up."*

### Purifying Drop *(Consumable item)*
- **Found:** Floor 3 loot (2×)
- **Effect:** Removes 4 Taint from one character. Does NOT restore HP (not a healing item).
- **Lore:** Condensed from the same blue medicinal leaves as a Healing Drop, but treated differently — the process extracts the purifying compounds instead of the restorative ones. Ellen would know how to make them. She probably already has.
- **Flavor text:** *"Different process. Same plant. Ellen could make these in her sleep."*

---

## IMPLEMENTATION ORDER (Recommended)

1. **Remove Nate's Radiant Heal, add Radiant Shield** — immediate lore fix
2. **Update Avery's Purify skill** — make it cleanse + small restore
3. **Veil Pressure** — highest impact, enables all other mechanics
4. **Synergy Combos** — rewards smart play
5. **Level-Up System** — progression backbone
6. **Enemy Scaling** — keeps fights relevant
7. **Positioning** — tactical layer
8. **Taint Accumulation** — long-term meta-game

---

## NOTES FOR GEMINI (Systems/Architecture Tasks)

When implementing these mechanics:
- Keep battle state object clean — add `veilPressure`, `taint`, `position`, `comboHistory` fields
- Use data-driven approach: skills defined in objects, not hardcoded
- Make Veil Pressure visible in UI — bar or gauge during battle
- Combo system should check AFTER action resolves, not before
- Level-up should pause the game briefly for celebration
- Taint is a per-character counter, not global
- Enemy scaling uses floor number from state object
- All new mechanics should work with existing turn queue system
