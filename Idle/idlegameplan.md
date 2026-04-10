# Echoes of the First Breach — Game Design Document

> *Alliance faces the Deepborn Awakening*
> Genre: **Idle RPG + Visual Novel** — with a real ending
> Protagonist: **The Commander** (player character, war coordinator of the Night Crystal Alliance)
> Status: Design locked — ready for implementation planning

---

## What This Game Is

An idle RPG where the player commands the Night Crystal Alliance against the Deepborn awakening. Resources tick passively, heroes fight automatically, you tap to accelerate — but the spine of the game is a **story that ends**. Every 25–50 stages, a visual novel scene plays. The Commander makes decisions. The story advances. When the Deep Lord is defeated, the game concludes with a real ending.

**The differentiator:** Almost no idle RPGs have a true ending. This one does.

---

## Genre DNA

| Mechanic | Source |
|---|---|
| Tap to deal damage, heroes deal passive DPS | Tap Titans / AFK Arena |
| Stage progression, boss every N stages | Tap Titans |
| Offline progress (AFK gold earned) | AFK Arena |
| Visual novel story scenes at milestone stages | Fate/Grand Order, Arknights |
| Commander decisions that change stats | Fire Emblem, XCOM |
| A real ending | Universal Paperclips |

---

## Platform Recommendation

**Primary: Godot 4** — idle + VN hybrid, Android/iOS/Web export from one codebase.
- GDScript maps well to idle game logic (timer nodes, signal-based systems)
- Dialogic plugin handles VN scenes natively (portraits, dialogue boxes, choices)
- HTML5 export works for browser version; APK for Android

**Prototype path:** Plain HTML/JS single-file prototype (matching existing Lore Navigator games) to validate core loop before committing to Godot.

---

## The Commander

The player character. A frontline fighter and war leader — the Commander is in every battle, not watching from behind.

- **Name:** Player-defined at game start (default: "Commander")
- **Combat role:** The Commander's tap = their personal attack. They are the sword. Heroes fight alongside, not instead.
- **Not a mage.** The Commander has no innate magic. Non-mage fighters are well-established in Kalmirth — the Black Leaf's entire force was non-mage Mage Hunters; most of Blue River Village had no magic; bounty hunters and soldiers exist throughout Night Etdler. The Commander fights with skill, discipline, and earned weapons. Allies respect this — the Commander leads without the crutch of power.
- **Combat Style:** Chosen in the tutorial VN scene:
  - *Blade* — physical swordfighter; high tap damage, lower passive; scales best with physical heroes (Druids)
  - *Vanguard* — the Commander fights at the front as a living shield; lower tap damage but each tap generates a block that reduces incoming Deepborn boss damage for all heroes for 2 seconds; scales with hero count
  - *Tactician* — lower tap damage, but each tap places a tactical marker that buffs all heroes DPS +15% for 2 seconds; scales with large rosters; hardest early, strongest late
- **Commander Stats:**
  - **Attack** — base tap damage; upgradeable
  - **HP** — Commander has health; if depleted, they retreat (lose the stage attempt, regenerate over time)
  - **Rally** (active skill) — boosts all hero DPS for 10 seconds; cooldown shrinks as Morale rises
  - **Resilience** — reduces HP loss from Deepborn counterattacks during boss fights
- **Level:** Commander levels up like heroes; each level increases all stats + unlocks a new skill slot
- **Equipment:** Weapon slots — the Commander upgrades their personal weapon through Alliance crafting (conventional forging, not Artifact rituals). High-tier weapons can be enhanced with faction magic materials (light-tempered steel, root-woven hafts, fire-forged blades) without involving the Black Leaf's sacrifice method.
- **Visual Novel role:** Protagonist in all VN scenes; silhouette portrait (no fixed face — player projection); speaks in first person; decisions affect stats
- **Story arc:** Begins as a proven but unproven war leader; by Act 5 has fought through the Underworld itself and carries the weight of every choice made along the way

---

## Core Game Loop

```
[TAP → Commander attacks the Deepborn enemy]
        ↓
[Tap damage + hero passive DPS → enemy HP depletes → enemy dies → earn Gold]
        ↓
[Enemies respawn; kill enough to advance the stage counter]
        ↓
[Spend Gold → upgrade Commander / hire faction heroes / upgrade heroes]
        ↓
[Heroes fight passively → stage advances even while AFK]
        ↓
[Stage boss every 25 stages → 30-second timer, fail = stay on stage]
     Boss fight: Commander + all heroes vs. mini-boss Deepborn unit
     Commander's Rally skill is the key burst button on boss stages
        ↓
[Milestone stage → VN scene plays (game pauses)]
     Commander speaks, other characters appear as NPCs
     Decision at scene end → affects Morale / Trust / stats
        ↓
[Continue tapping / go AFK → offline progress on return]
        ↓
[Narrative Prestige at Act end → story advances, permanent buffs, restart faster]
```

**Key distinction from pure idle:** The Commander is always in the fight. Tapping is personal — it represents the Commander cutting through Deepborn personally. The tactile loop matters. Heroes amplify; the Commander leads.

---

## Strategy Layer — Stats the Commander Manages

These are not just idle numbers. The Commander's VN decisions and hero choices shape them.

### Alliance Morale (0–100)
- Below 30: heroes deal −25% DPS; events become harder
- Above 80: heroes deal +15% DPS; trust restoration events trigger
- Sources: VN scene good outcomes, winning boss fights, recruiting heroes
- Drains: losing a boss fight, choosing a faction over another in events, zone falling

### Faction Trust Web (10 pairs, each 0–100)
Same structure as before but **Commander decisions in VN scenes** are the primary shifter — not just building choices.

| Pair | Starting | Main Tension |
|---|---|---|
| Light ↔ Dark | 20 | Light Mages see Dark magic as Deepborn-adjacent |
| Nature ↔ Fire | 30 | Fire Mages destroy forests |
| Light ↔ Dark | 20 | The core political crisis of Act 3 |
| All others | 40–85 | See full table in Faction System section |

**Trust effects:**
- 80+: Synergy DPS bonus when both factions deployed to same zone
- 50–79: Neutral
- <30: Faction passive generation reduced; joint events fail automatically
- <10: Faction threatens to leave — Commander must spend Morale to retain them

### Corruption Pressure (per zone, 0–100%)
- Passively increases each stage; rate accelerates after Act 3
- At 100%: zone falls, all heroes deployed there lose 50% HP temporarily
- Managed by deploying faction defenders (passive mitigation) and winning boss fights (resets zone 20%)

### Resource Rates
- **Gold**: tap + heroes → used for all upgrades
- **Crystal Shards**: rare, from bosses + Origin events → used for tier-3 upgrades + prestige perks
- **Essence** (faction-specific): Light/Root/Shadow/Ember — used for faction-specific hero upgrades

---

## Hero Roster

Heroes are **original characters** from each faction — not the main story cast. Avery, Nate, Callen, Silene, Thunder, Zade, Fran, Grace, Faeron, and Callen appear only as **NPCs in VN scenes**. They are important to the Commander, but they are not yours to deploy.

The heroes you command are soldiers, mages, and fighters from each faction's ranks — people the Commander meets, recruits, and fights alongside.

### Light Mages — "The Steadfast"
| Hero | Role | DPS Type | Unlock | Special |
|---|---|---|---|---|
| **Warden Sael** | Shield-bearer | Tanking + steady passive | Act 1 Stage 5 | Passive: absorbs 10% of all incoming Deepborn damage directed at Commander |
| **Archivist Veth** | Lore mage | Support + buff | Act 1 Stage 15 | Active: buffs all Light heroes +30% DPS for 8s; 3-min cooldown |
| **Beacon Lyss** | Healer | Healing aura | Act 1 Stage 30 | Passive: restores Commander HP 2% per second when below 40% |

### Nature Mages / Dryad — "The Rooted"
| Hero | Role | DPS Type | Unlock | Special |
|---|---|---|---|---|
| **Rootwarden Aela** | Earth defender | Zone protection | Act 2 Stage 55 | Passive: reduces Corruption rate in assigned zone by 15% |
| **Bloom Keeper Myr** | Life mage | Healing over time | Act 2 Stage 65 | Passive: all heroes regenerate 1% HP/second outside boss fights |
| **Briar Sentinel Olin** | Ambush fighter | Burst on corrupted enemies | Act 2 Stage 80 | Deals 2× DPS against enemies that have Corruption debuff |

### Shapeshifter Druids — "The Changing"
| Hero | Role | DPS Type | Unlock | Special |
|---|---|---|---|---|
| **Ironpelt Bor** | Bear-form brawler | Heavy melee burst | Act 2 Stage 60 | Active: bear charge triples his DPS for 12s; 4-min cooldown |
| **Swiftclaw Narra** | Wolf-form striker | Fast multi-hit passive | Act 2 Stage 75 | Passive: attacks 4× per second (lower per-hit, high total) |
| **Stoneback Hael** | Turtle-form tank | Damage reduction | Act 3 Stage 140 | Passive: reduces all boss attack damage by 20% while alive |

### Dark Mages — "The Misunderstood"
| Hero | Role | DPS Type | Unlock | Special |
|---|---|---|---|---|
| **Shade Wren** | Shadow assassin | Highest single-target | Act 3 Stage 115 | Passive: deals bonus damage equal to 10% of enemy's remaining HP |
| **Veilmage Sorn** | Void caster | AoE slow + damage | Act 3 Stage 130 | Active: voids an area — slows all enemies −30% attack for 10s |
| **Whisper Kael** | Illusion mage | Support / decoy | Act 3 Stage 155 | Active: summons decoy that absorbs 2 boss attacks before dissolving |

### Fire Mages — "The Burning"
| Hero | Role | DPS Type | Unlock | Special |
|---|---|---|---|---|
| **Ashwalker Drin** | Burn specialist | Sustained DoT damage | Act 3b Stage 170 | Passive: enemies hit by Commander tap gain burn (5% DPS per second, 5 ticks) |
| **Emberbrand Torr** | Explosive caster | Burst + backfire risk | Act 3b Stage 185 | Active: massive AoE burst; 20% chance to also deal 10% damage to one random allied hero |
| **Cinderborn Vel** | Area fire | Zone-cleansing AoE | Act 4 Stage 220 | Passive: reduces Corruption in all zones −5% after every boss kill |

---

## Main Cast — NPC Roles Only

These characters appear in VN scenes, shape the story, and speak to the Commander. They are never deployed as heroes.

| Character | Faction | NPC Role in Story |
|---|---|---|
| **Avery** | Crystal Origin | The Commander's key ally; her Crystal power is the weapon against the Deep Lord; central to all endings |
| **Nate** | Light Mages | Light Mage heir; first to trust the Commander; tests the Commander's willingness to sacrifice |
| **Callen** | Alliance / Mage Hunters | Strategic advisor; gives the Commander hard truths; knows more than he says |
| **Silene** | Nature/Dryad | Nature Mage leader; slow to trust; her archive contains the Great War truth |
| **Thunder** | Druids | Druid leader; his promise subplot tests whether the Commander keeps their word |
| **Zade** | Dark Mages | Approaches the Commander alone; the recruit-or-refuse decision belongs to his scene |
| **Fran** | Physical Origin | Arrives mid-game; kept her memories; her presence is a morale event |
| **Grace** | Unknown Origin | Late arrival; can perceive the Deep Lord's movements; unlocks the Outsider subplot |
| **Faeron** | Elder Elf | Appears and disappears; the only living elf who knows why the Deepborn woke | 
| **The Outsider** | Deepborn (conscious) | Encountered in Act 4; the moral question of the game: ally, prisoner, or enemy? |

---

## Faction System

Five factions, unlocked through story progression. Each has three building tiers (passive resource / zone defense).

### 1. Light Mages
- Color: `#f0c940`
- Buildings: Sanctuary → Beacon Tower → City of Blinding Light
- Passive: Alliance Points + Corruption suppression
- Act unlock: **Start**

### 2. Nature Mages / Dryad
- Color: `#5caa5c`
- Buildings: Glade Shrine → Root Network → Wrysse Heart
- Passive: Exponential Root Pulse scaling; slow start
- Act unlock: **Act 2** (Stage 50)

### 3. Shapeshifter Druids
- Color: `#b08030`
- Buildings: Wolf Den → Shapeshifter Grove → Kryonair Stronghold
- Passive: Combat event multiplier
- Act unlock: **Act 2** (Stage 50, alongside Nature)

### 4. Dark Mages
- Color: `#7040a0`
- Buildings: Shadow Cache → Void Archive → Black Forest Enclave
- Passive: Highest passive generation; each build costs Light Trust
- Act unlock: **Act 3** — requires Commander decision in VN scene
- Lore reveal: Dark Mages were falsely blamed; Commander learns this in VN scene

### 5. Fire Mages
- Color: `#e05020`
- Buildings: Ember Camp → Flame Forge → Inferno Bastion
- Passive: High burst; 15% backfire risk per event
- Act unlock: **Act 3b** — Dark Mages must reach 40 trust first

---

## Zones (Corruption Map)

Five zones. The SVG map of Night Etdler shows corruption spreading as color wash.

| Zone | Region | Unlocks | Corruption Rate |
|---|---|---|---|
| Blue River Valley | Blue River | Act 1 start | Slow (1/stage) |
| Vleyrra Forest | Night Etdler | Act 2 | Medium (1.5/stage) |
| Huge Woods | Night Etdler | Act 2 | Medium (1.5/stage) |
| Kryonair/Wrysse | Night Etdler | Act 3 | Fast (2/stage) |
| Underworld Threshold | Below Night Etdler | Act 4 | Very fast (3/stage) |

---

## Story Beats — Full VN Scene List

The Commander is present in every scene. Each scene ends with a **Decision** that shifts stats.

---

### ACT 1 — Blue River Falls (Stages 1–75)
*The Commander first hears that something is wrong underground.*

**[Stage 10] — "The River Dims"**
- Setting: Alliance council chamber
- Beat: Avery arrives with a warning. The Blue River is losing its glow. Something is spreading below.
- Commander decision: *"Do we send scouts now, or wait for more evidence?"*
  - Scout now: −50 Gold, +10 Morale (decisive action)
  - Wait: +30 Gold, −5 Morale (cautious but loses early warning)

**[Stage 25] — "First Contact"**
- Setting: Edge of Vleyrra Forest
- Beat: Scouts return. A Deepborn horde was found 3 kilometers underground. Small, mindless. But moving upward.
- Commander decision: *"Report to all five factions or keep it quiet to avoid panic?"*
  - Report all: −10 Trust all factions (fear response), +15 Morale (transparent)
  - Keep quiet: +0 Trust, −15 Morale (Commander carries it alone)

**[Stage 50 — ACT 1 BOSS] — "Blue River Burns"**
- Setting: Blue River Village in flames
- Beat: The mindless horde surfaces. Blue River Village is attacked. Avery's hometown. Commander must decide how to deploy the first defense.
- Commander decision: *"Prioritize civilian evacuation or hold the line for the village?"*
  - Evacuate: Village is lost. −20 Morale (heavy). All civilians survive. Nate's trust +10.
  - Hold the line: Some civilians lost. −10 Morale. Village partially saved. Nate's trust −5 (he saw the cost).
- PRESTIGE 1 UNLOCKED: *"The Alliance mobilizes. This is no longer a warning. The war has begun."*
- Permanent unlock: Nate hero card

---

### ACT 2 — The Alliance Forms (Stages 76–200)
*The Commander builds the Alliance. Not every faction trusts the others — or the Commander.*

**[Stage 80] — "Into Vleyrra"**
- Setting: Vleyrra Forest edge, first meeting with Silene
- Beat: Nature Mages are reluctant. The last war showed them what happens when they trust outsiders. Silene tests the Commander's intentions.
- Commander decision: *"Acknowledge the elves' historical betrayal by humans, or avoid the topic?"*
  - Acknowledge: +20 Nature Trust, −5 Light Trust (some Light Mages uncomfortable)
  - Avoid: +5 all Trust (safe), Silene notes it — memory recorded, will surface in Act 3

**[Stage 95] — "Thunder's Price"**
- Setting: Kryonair Forest, Thunder's camp
- Beat: Thunder will join, but he has a condition: the Alliance must promise Druid territory is never sacrificed for strategic gain. The Commander must commit.
- Commander decision: *"Make the promise, or deflect with diplomatic language?"*
  - Promise: Druid Trust +30, but creates a binding obligation that will complicate Act 4's Underworld Threshold decision
  - Deflect: Druid Trust +10, Thunder notices — "A Commander who won't make hard promises won't keep them either"

**[Stage 120] — "The Uncomfortable Request"**
- Setting: Alliance HQ, night. Zade arrives alone.
- Beat: Zade makes the case for Dark Mage recruitment. Their magic is the most effective against the Deepborn's void corruption — shadow disrupts shadow. The Commander already knows the Light Mages will hate this.
- Commander decision: *"Recruit the Dark Mages openly, secretly, or refuse?"*
  - Openly: Dark Mages join; −30 Light Trust (they see it as a betrayal); Morale −15 (faction tension)
  - Secretly: Dark Mages join quietly; will be discovered in Act 3; creates a crisis
  - Refuse: Dark Mages do not join Act 2 — they can still join later, but Zade remembers the rejection

**[Stage 150] — "The Last Faction"**
- Setting: An open field. Fire Mage survivors around a broken camp.
- Beat: The Fire Mages' original leader Castor was killed. They're leaderless. One of them approaches the Commander — "We have no one. But we'll burn for you if you want us."
- Commander decision: *"Accept unconditionally, or set terms for Fire Mage conduct?"*
  - Unconditional: Fire Trust +30, Druid Trust −10 (Druids fear uncontrolled fire)
  - With terms: Fire Trust +20, Druid Trust neutral; Fire Mages respect the clarity

**[Stage 175 — ACT 2 BOSS] — "The First Organized Assault"**
- Setting: Huge Woods, all five factions present for the first time
- Beat: A coordinated Deepborn assault — not mindless this time. The horde knows where the Alliance is gathering. It attacks on three fronts simultaneously.
- Commander decision: *"Split forces to defend all fronts, or concentrate and let one front take damage?"*
  - Split: All zones hold. High casualties. Morale −20. Every faction's trust +5 (they see the Commander values everyone).
  - Concentrate: One zone falls temporarily. Morale −10. The faction whose zone fell: Trust −20.
- PRESTIGE 2 UNLOCKED: *"The Alliance of Five is forged. But something coordinated that attack. And it wasn't the mindless horde."*
- Permanent unlock: all five faction buildings available

---

### ACT 3 — The Truth Underground (Stages 201–375)
*The Commander learns what the war is really about. So does the Alliance — and it nearly breaks them.*

**[Stage 210] — "Faeron's Warning"**
- Setting: Alone in the forest. Faeron appears without warning.
- Beat: Faeron says one thing: "What is stirring below is not new. It was sealed once. The elves who broke that seal — they did not know what they were doing. But someone in your Alliance does." He walks away.
- Commander note: No decision. Just information. The Commander writes it down.

**[Stage 240] — "The Underworld Threshold"**
- Setting: A crack in the earth near Vleyrra's deepest roots
- Beat: Scouts found a labyrinth portal — a spatial distortion leading down. Time doesn't work normally inside. Grace (if recruited) confirms it: "I can feel the Deep Lord from here. It's awake."
- Commander decision: *"Send a scout team down, or seal the portal and focus on surface defense?"*
  - Send scouts: Lose 1 hero temporarily (they're disoriented by time distortion), but gain deep intel → all boss fights in Act 4 easier
  - Seal: Safe. But the Underworld continues to expand outward; Corruption rates increase 20%

**[Stage 270] — "Silene's Archive"**
- Setting: Wrysse Heart, Silene's inner library
- Beat: Silene shows the Commander the encoded human-side records of the Great War. The truth is there: the young elves broke the Deepborn seals. Not the humans. Not the Dark Mages. Desperate elves, losing a war, who didn't know what they were releasing.
- Commander decision: *"Tell the Alliance, tell only key leaders, or carry this alone?"*
  - Tell everyone: Massive trust collapse between human factions and elven factions (−25 Trust all elf-related pairs). +20 Morale eventually (the Commander chose honesty). Dark Mages vindicated: +40 Dark Trust with all.
  - Tell key leaders only: Controlled crisis. Less damage, but Light Mages find out later anyway → they feel manipulated when they do.
  - Carry alone: Commander holds this for Act 4. The weight costs Morale −30 over time but protects faction trust short-term.

**[Stage 300] — "The Fracture"**
- Setting: Alliance HQ, chaos
- Beat: The truth comes out — however the Commander handled Stage 270, it arrives. The Light Mages are furious. The Dark Mages are vindicated and furious that they were blamed. The Nature Mages are ashamed and defensive. The Commander is in the center.
- Commander decision: *"Call a formal Alliance tribunal, issue a unilateral statement, or let factions argue and wait?"*
  - Tribunal: Slow process; Trust recovers over time; Morale stable; delays Act 4 (harder Corruption pressure)
  - Statement: Commander takes a stance; one faction gains 30 Trust, one loses 30 Trust depending on what the Commander says
  - Wait: Worst short-term (Morale −30, Trust drops), but Morale eventually recovers higher once factions exhaust themselves

**[Stage 350 — ACT 3 BOSS] — "The Deep Lord Speaks"**
- Setting: Edge of the Underworld Threshold
- Beat: For the first time, the Deep Lord communicates — not in words, but in a vision felt by every mage in the Alliance simultaneously. It shows them the original sealing. It shows them the elves breaking it. It shows them that it knows the Commander is coming.
- Commander decision: *"Treat this as a threat, as a warning, or as a possible negotiation?"*
  - Threat: All factions rally (Morale +30, Trust +10 all) — fear unifies
  - Warning: The Commander begins to wonder if there's a path that isn't destruction. Sets up The Outsider subplot.
  - Negotiation: Faction leaders are divided; some see it as weakness; opens a late-game option
- PRESTIGE 3 UNLOCKED: *"The Alliance knows what it's fighting. It knows why. Now comes the question of whether knowing changes anything."*

---

### ACT 4 — The Descent (Stages 376–550)
*The Commander leads the Alliance into the Underworld. Space and time do not work normally here.*

**[Stage 385] — "The Offer"**
- Setting: Alliance HQ, late night. A Black Leaf remnant defector arrives.
- Beat: They offer to teach the Alliance the Artifact ritual — "you need weapons strong enough to hurt the Deep Lord; we know how to make them. Seal a mage's life into the weapon. We've done it for years." Every faction leader is in the room. The silence is heavy.
- Commander decision: *"Accept the knowledge, refuse outright, or hear them out and decide later?"*
  - Accept: Faction weapons gain a temporary power tier. Callen leaves the room without a word. Nate doesn't look at the Commander for ten stages. Dark Mages, who have been falsely blamed for things almost as bad, are quiet. Morale −25. All Trust −10.
  - Refuse: The defector is dismissed. The Alliance finds another way. Act 4 and 5 are harder but all faction Trust +15. The Commander holds the line on what the Alliance is.
  - Hear them out: The defector explains the full ritual, including the Final Mission — Black Leaf members will kill themselves when magic is gone to destroy the sealed power inside them. The Commander now knows the full shape of what they were offered. The choice comes back in Stage 420.

**[Stage 420] — "The Outsider"**
- Setting: Deep in the Underworld labyrinth
- Beat: A Deepborn with individual consciousness approaches. It doesn't attack. It speaks: "We are not what you think we are. The Deep Lord is not our will — it is our prison." It wants to help.
- Commander decision: *"Trust it, imprison it, or destroy it?"*
  - Trust: The Outsider becomes an ally. Grace can communicate with it. Opens a secret path in Act 5.
  - Imprison: Safe option. The Outsider's knowledge is extracted. Less powerful than trusting it but no risk.
  - Destroy: The Alliance sees the Commander as decisive. +15 Morale. But a possible ally is gone, and something in Avery's expression says the Commander made a mistake.

**[Stage 480] — "Thunder's Promise Comes Due"**
- Setting: Deep in the labyrinth. The Commander realizes the only path forward goes through Druid territory — and the Deepborn will consume Kryonair Forest in the process.
- Beat: If the Commander promised Thunder at Stage 95 that Druid territory would never be sacrificed — that promise breaks here. Thunder arrives.
- Commander decision: (only if promise was made) *"Break the promise and explain why, or find another way that costs more lives?"*
  - Break promise: Druid Trust −40. Thunder leaves the front (temporarily). Morale −25. The Commander earns a reputation for hard choices.
  - Alternative path: More heroes fall. Morale −30 from losses. Promise kept. Thunder stays. Costs more.
  - (If no promise was made): Thunder still confronts the Commander, but the cost is −10 Trust, not −40. Decisions compound.

**[Stage 525 — ACT 4 BOSS] — "The Inner Chamber"**
- Setting: The Deep Lord's outer sanctum
- Beat: The Alliance breaches the labyrinth's deepest level. The Deep Lord is visible — an ancient intelligence filling an entire cavern. It is not mindless. It has been watching the Commander the entire game.
- Commander decision: *"Attack immediately, or give Avery time to analyze its structure first?"*
  - Attack immediately: Avery doesn't get her analysis. Act 5 is harder but faster.
  - Wait for Avery: Avery discovers a structural weakness. Act 5 gets a 30% damage bonus. But during the wait, two zones take heavy Corruption damage.
- PRESTIGE 4 UNLOCKED (FINAL): *"The Alliance stands at the threshold. There is no return from this. The Commander leads them down."*

---

### ACT 5 — The First Breach (Stages 551–750)
*The final act. The game ends here.*

**[Stage 560] — "What It Said"**
- Setting: The Commander alone, briefly, before the final advance.
- Beat: The Deep Lord spoke to the Commander in Stage 350. The Commander has carried what it said. This scene reveals it: *"You sealed us before by forgetting us. If you seal us again — the one who seals it cannot leave."*
- Commander monologue: No decision. Just the Commander thinking.

**[Stage 600] — "Avery's Question"**
- Setting: Quiet moment in the labyrinth. Avery finds the Commander.
- Beat: "I heard it too, you know. When you sealed us before… Do you know what it means? One of us might not make it back."
- Commander decision: *"Tell her the truth, or protect her from it?"*
  - Tell her: Avery makes her own choice to be the one who seals it. She has the Crystal magic — she can seal herself in crystal and still survive (Book 1 lore: Avery seals herself in crystal at the story's end). She volunteers.
  - Protect her: The Commander carries the burden. The Commander may be the one who doesn't return — this affects which ending the player gets.

**[Stage 650] — "The Alliance's Last Stand"**
- Setting: All five factions arrayed in the Deep Lord's sanctum
- Beat: Every faction leader speaks. The moment before the final battle. Thunder. Silene. Zade. Nate. A Fire Mage whose name the Commander finally learned. And the Commander.
- No decision — just character portraits, dialogue, and the weight of everything that led here.

**[Stage 700] — "The Outsider's Gift"**
- Setting: The Outsider appears (only if trusted in Stage 420)
- Beat: The Outsider reveals the precise weakness — a conduit in the Deep Lord's consciousness that only Crystal magic can touch. Avery knows what to do. This unlocks the True Ending path.

**[Stage 750 — FINAL BOSS] — "The Deep Lord"**
- The hardest fight in the game. All five factions must be active. Trust must average 50+ across all pairs. Boss timer: 60 seconds (longer than all previous bosses).
- If the Commander has high Morale and good trust: bonus DPS from Alliance unity.
- If trust is fractured: some heroes fight at reduced capacity — the Alliance is not fully one.

---

### ENDING — "The First Breach, Sealed"

*Three possible endings based on the Commander's choices across all acts.*

**Ending A — The Commander's Seal** (Avery protected from truth at Stage 600)
- The Commander seals the breach — physically, standing in the collapse as the Alliance pulls back.
- The Commander does not return from the Underworld.
- The Alliance holds. The river runs bright.
- Final image: The five faction leaders looking at the sealed breach. Nate holds Avery's hand. Neither speaks.
- Closing line: *"They never learned the Commander's name. There was no grave to mark. The river just... stayed bright."*

**Ending B — Avery's Crystal** (Avery chose to seal it; Outsider trust unlocked)
- Avery seals herself in crystal within the breach. Lore-canon ending.
- She is not gone — she is the seal.
- The Commander leads the Alliance back to the surface.
- Final image: The Commander at Blue River, which is glowing again.
- Closing line: *"The war cost things that cannot be returned. But the cost was chosen, not taken. There's a difference. The Commander knew it. So did she."*

**Ending C — Negotiated Peace** (Deep Lord treated as warning at Stage 350; negotiation pursued)
- Only available if The Outsider was trusted AND the Commander chose to negotiate
- The Deep Lord is not destroyed — it is re-sealed with consent. The Outsider mediates.
- The Deepborn are contained, not exterminated. Some can be helped.
- Final image: The Outsider standing at the entrance to the Underworld Threshold, which is now a door, not a wound.
- Closing line: *"There was a fourth option no one expected: understanding. The Commander found it."*

---

## Prestige System — Narrative Acts

Each prestige is a **story act transition**, not just a mechanical reset.

| Prestige | Story Beat | Keeps | Resets | Permanent Bonus |
|---|---|---|---|---|
| Act 1→2 | "The war has begun" | Avery hero, all VN scenes seen | Gold, buildings, stages | +5% global Gold gain |
| Act 2→3 | "The Alliance of Five is forged" | All heroes, all seen scenes | Gold, buildings | +10% hero DPS base |
| Act 3→4 | "What the war is really about" | Heroes, Crystal Shards | Gold, buildings | +15% Crystal Shard drop |
| Act 4→5 | "The descent begins" | Everything except stages | Gold, buildings | +20% all DPS; Acts go faster |

**Crystal Memories** (carried through all prestiges): spent on prestige-only upgrades in a "Commander's Memory" panel.

---

## VN Scene Technical Design

Each VN scene:
- Full-screen overlay on top of the idle game (game pauses)
- Character portrait left side (from `Character Concept/` images)
- Commander silhouette right side (no face)
- Dialogue box: Cinzel speaker name + Crimson Pro text (matching all Kalmirth games)
- Decision buttons appear at scene climax: 2–3 options, each with stat preview
- Stat changes shown briefly after choice: `[ Morale −15 ] [ Light Trust +10 ]`
- Skip button available (for replays)
- All 23 scenes + 3 endings = one complete playthrough of approximately 4–6 hours

---

## Lore Fragment System

Short 2–3 sentence unlocks that appear as toasts at milestone moments (not VN scenes — faster, quieter):

| Trigger | Fragment |
|---|---|
| First hero purchased | *"The Alliance is not a treaty. It is a choice made again every morning."* |
| Zade recruited | *"Shadow is not Deepborn. Shadow is what you see when light reaches its limit."* |
| The Offer refused | *"The Commander said no. Everyone in that room remembered it."* |
| Faeron appears | *"He was the last elf who remembered. He never told anyone. He thought it would die with him."* |
| The Outsider encountered | *"Something stirred in the labyrinth. Not hostile. Watching. Waiting to see what the Commander would do."* |
| Black Leaf Final Mission learned | *"They planned to die by their own weapons when the world was clean. They believed that was courage. The Commander wasn't sure what it was."* |
| 100% Trust any pair | *"Two factions who once refused to share a field now share a front line. The Commander made that happen."* |
| Ending A unlocked | *"The Commander never came back. The river stayed bright. That was enough."* |

---

## UI Layout

```
┌────────────────────────────────────────────────────────────────┐
│  [Gold: 12,450/s]  [Morale: 72 ██████░░]  [Stage: 147]  [TAP]  │
├─────────────────┬──────────────────────────┬───────────────────┤
│  HERO ROSTER    │    SVG MAP (Night Etdler) │  STAGE FEED       │
│                 │   ┌────────────────────┐  │                   │
│ [Avery  Lv.24]  │   │  [zone shapes +    │  │ Stage 147         │
│ [Nate   Lv.18]  │   │   corruption wash] │  │ ▓▓▓▓▓▓░░░ 67%    │
│ [Silene Lv.12]  │   │  [click crystal]   │  │                   │
│ [Zade   Lv. 8]  │   └────────────────────┘  │ Next boss: 150    │
│                 │  Corruption: Vleyrra 44%   │ 3 stages away     │
│ [+ hire hero]   │  Blue River: 12%           │                   │
│                 │                            │ [SKILL: Nate]     │
│ Trust Web ▼     │                            │ [SKILL: Thunder]  │
└─────────────────┴──────────────────────────┴───────────────────┘
```

---

## Tech Architecture

**Engine:** Godot 4 (recommended) or plain HTML/JS (prototype)

**If HTML/JS:**
- Single `index.html` in `Lore Navigator/Idle/`
- `setInterval` game loop at 100ms (10 ticks/s)
- VN scenes as full-screen overlay divs, fade in/out
- All portraits from `../Character Concept/` folder

**Save State (localStorage key: `kalmirth_idle`):**
```js
{
  act: 1,         // current act (1–5)
  stage: 1,       // current stage
  prestige: 0,    // prestige count
  gold: 0,
  crystalShards: 0,
  crystalMemories: 0,
  morale: 75,
  heroes: { avery: { level: 1, unlocked: true }, ... },
  factions: { lightMages: { trust: 100, buildings: [0,0,0] }, ... },
  trust: { lightNature: 70, lightDruid: 65, ... },
  zones: { blueRiver: { corruption: 0, fallen: false }, ... },
  vnSeen: { s10: false, s25: false, ... },
  decisions: { s10: null, s25: null, ... },  // tracks choices for ending calc
  loreFragments: {},
  ending: null,  // A / B / C — set on game completion
  lastSaved: 0
}
```

**Offline progress:**
```js
const elapsed = Date.now() - state.lastSaved;
const missedTicks = Math.min(elapsed / 100, 8 * 60 * 60 * 10); // cap 8h
applyTicks(missedTicks);
```

---

## Remaining Open Questions

1. **Real-time or turn-based?** Resources tick while tab is open (real-time) vs. each stage is one "turn" (strategic pacing). Real-time is standard for idle games; turn-based would make it feel more like a war game.
2. **Tap target?** Click the screen / a crystal / the enemy currently on stage? All equivalent mechanically — visual choice.
3. **Mobile-first or PC-first?** Other Lore Navigator games are PC. Godot path enables mobile naturally.
4. **Voice/sound?** The walking-sim games are silent. Should this one have ambient audio or simple SFX?
