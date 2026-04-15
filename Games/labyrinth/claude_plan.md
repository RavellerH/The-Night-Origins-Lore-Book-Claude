# Labyrinth — Claude Session Log

> **Session date:** 2026-04-14
> **Claude role:** Story / Dialogue / Lore Canon / Cutscene System

---

## What Claude owns in this project

3-agent split per ARCHITECTURE.md (updated 2026-04-14 by Qwen):

| Area | Owner |
|------|-------|
| Story beats, dialogue, lore text | **Claude** |
| ENEMY_TYPES lore, skill descriptions | **Claude** |
| Focus Item / Rune Fragment flavor text | **Claude** |
| Lore canon verification | **Claude** |
| Cutscene content (captions, choices, notes) | **Claude** |
| All CSS, visual design, animations, canvas art | **Gemini** |
| Cutscene CSS → needs Gemini review | **Gemini** |
| Engine code, battle systems, JS logic, floor loading | **Qwen** |
| Cutscene JS engine → needs Qwen review | **Qwen** |

**Note on cutscene session work:** Claude wrote both the engine (`showCutscene`, `resolveCutsceneChoice`) and CSS for the cutscene overlay — outside Claude's lane. Content is correct and lore-accurate. Qwen should review/own the JS engine; Gemini should restyle the CSS to match their visual standards.

---

## Session 1 — 2026-04-14

### Canon Audit (Floors 2-4)

Checked all Floor 2-4 characters against `Lore/CHARACTERS.md` before writing a line.

**Cleared:**
- Alyse Lucent is alive during the Labyrinth (Ch. 63 = Part 2; she dies after Zachariel's attack, which is Part 2 end). No conflict.
- Alyse makes light butterflies (script-verified). Her "Light Support" role is canon-accurate. Her skills must NOT include healing — light magic = offensive/support only.
- The Twin Aura sensation on Floor 4 must NOT name Fran. Avery feels "something like herself from below." Identification happens at Floor 8 when Lynne speaks.

**Flags resolved:**
- Frank Dewflame's light magic is weak — used as fuel for machines, not combat. His skills are gadget/machine based (Light Gun, Deploy Trap, Light Fuel, Gadget Shield). Zero standard mage attacks.
- Ellen Shadowseer is an herbalist first. Her skills are item-efficiency (Infuse Herb: +60% item effect) + protective light buffs (Light Ward, Steady Hands, Tonic). Tonic removes Taint without consuming items — distinct from Crystal Purify.
- Crystal Purify narrative-gameplay mismatch resolved: skill exists in Avery's data from Floor 1 but is locked (`unlocked: false`) and hidden from the skill menu until the Floor 3 Alyse scene completes.

---

### Character Skill Specs — written to `BATTLE_MECHANICS.md`

#### Frank Dewflame

| Skill | Type | Effect | MP | Veil |
|-------|------|--------|----|------|
| Light Gun | Attack | Single-target light dmg (power 1.4) | 6 | -5% |
| Deploy Trap | Delayed | Next enemy to act takes bonus dmg (power 1.2) | 8 | — |
| Light Fuel | Buff | One ally ATK +30% for 2 turns | 10 | — |
| Gadget Shield | Buff | Party physical DEF +20% for 2 turns | 12 | — |

Veil note: Light Gun reduces veil only -5% (vs. Nate's -12%) — his light is weak and machine-routed.

#### Ellen Shadowseer

| Skill | Type | Effect | MP |
|-------|------|--------|----|
| Infuse Herb | Restore | Uses 1 Healing Drop from inventory with +60% effect | 5 |
| Light Ward | Buff | Party -15% damage taken for 2 turns | 8 |
| Steady Hands | Buff | One ally: next skill guaranteed accuracy | 6 |
| Tonic | Restore | Removes 2 Taint from one ally, no item consumed | 10 |

---

### Crystal Purify Lock — written to `BATTLE_MECHANICS.md` → MECHANIC LOCKS

- `unlocked: false` added to Purify skill object in `PARTY` data
- `showSkillMenu()` now filters `s.unlocked !== false`
- `unlockCrystalPurify()` function added — call this from the `alyse_names_it` dialogue callback when Floor 3 is built

**Gemini task:** wire `unlockCrystalPurify()` into the Floor 3 dialogue trigger chain.

---

### Dialogue Beats — written to `STORY.md`

Full JS-ready `STORY_BEATS` entries for Floors 2, 3, 4 with trigger and chain order documented.

**Floor 2 — Frank:**
`frank_found` → `frank_speaks` → `frank_response` → `frank_shadows` → Frank joins party

**Floor 3 — Alyse:**
`alyse_found` → `alyse_voice` → `alyse_taint` → `avery_afraid` → **[alyse_cutscene fires here]** → `avery_purifies` → `alyse_names_it` (callback: `unlockCrystalPurify()`) → Alyse joins party

**Floor 4 — Ellen:**
`ellen_found` → `ellen_speaks` → `ellen_nate` → **[ellen_cutscene fires here]** → `avery_twin_aura` → `avery_silent` → Ellen joins party → Floor 4→5 transition

---

### Cutscene System — implemented in `index.html`

New state: `cutscene` (added to state machine).

**UI:** Full-screen overlay (`z-index: 50`). CSS gradient image placeholder (real art from `ART_PROMPTS.md` when generated). Italic caption. 3 choice buttons — styled with Cinzel shortcut keys.

**Controls:** `1`/`2`/`3` direct pick, `↑`/`↓` navigate, `Enter` confirm. 1.4s outcome note shown, then returns to `explore`.

**5 cutscenes implemented:**

| ID | Trigger | Choices → Mechanical effect |
|----|---------|------------------------------|
| `portal_entry` | Floor 1 start (live) | Cautious → DEF+15% \| Urgent → SPD+15% \| Crystal instinct → Shard power +0.3 |
| `frank_cutscene` | Floor 2 Frank found | Reassure → ATK+10% \| Intel → Veil start -10% \| Build something → +1 Deploy Trap |
| `alyse_cutscene` | Floor 3 purification pivot (between `avery_afraid` and `avery_purifies`) | All-in → -12HP + MAG+20% + free Purify \| Measured → clean unlock \| With Nate → -2 Taint each + auto-shield next battle |
| `ellen_cutscene` | Floor 4 after `ellen_nate` beat | Silent → DEF+15% \| Intel → hidden chest revealed \| Acknowledge → -1 Taint all |
| `convergence_cutscene` | Floor 8 Fran meeting | Who are you? → standard \| "I felt you Floor 4" → Prismatic +5% \| "You're an Origin" → Crystal Resonance unlocked |

**Canon rule enforced in all cutscenes:** choices change mechanical outcomes only. Story events are fixed — Avery always finds Frank, Alyse, Ellen. The convergence always happens. Lynne always identifies them.

---

## Open Items for Next Claude Session

- [ ] Floor 5-7 dialogue beats (Fran's POV — Myst invisible, Lynne's "Broken Seals" lore reveal)
- [ ] Floor 8 convergence scene dialogue ("What?!" moment, Lynne's identification line)
- [ ] Rune Fragment collectible texts (3 fragments: Seal, Time, Origin — structure in `STORY.md`)
- [ ] Epilogue dialogue — Siege of Light scene, Avery's exit narration
- [ ] Canon audit for Fran's party: Myst (invisible, large), Lynne (orange-red curls, elf, nature magic unspecified) — skill specs needed before Chapter 2 is built

## Pending from Gemini (do not duplicate)

- [ ] Multi-floor architecture (`loadFloor(n)`, `MAPS` object)
- [ ] Wire `frank_cutscene` trigger to Floor 2 map
- [ ] Wire `alyse_cutscene` → `unlockCrystalPurify()` to Floor 3 dialogue chain
- [ ] Wire `ellen_cutscene` trigger to Floor 4 map
- [ ] Wire `convergence_cutscene` trigger to Floor 8 map
- [ ] Inventory screen (I key)
- [ ] XP + leveling system
- [ ] Enemy scaling per floor
- [ ] Taint accumulation logic (UI exists, per-battle increment missing)
