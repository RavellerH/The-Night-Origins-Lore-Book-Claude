# Callen's Training — Beat 'Em Up Plan

> Based on Book 1 lore  
> You are Callen. Two years of training under Zachariel. The man who killed your parents. But you don't know that yet.

---

## Lore Context

After the Blue River assault, Callen (16) is captured by Zachariel and brought to Aysel Village. For two years, he trains as a Mage Hunter under Zachariel — the man who froze his adoptive parents in crystal. Callen doesn't know who Zachariel really is. He's learning to fight. He's learning to kill. He's becoming something he doesn't recognize.

**Dark twist:** The player realizes halfway through who Zachariel really is. The training montage becomes a tragedy.

---

## Game Concept

**Side-scrolling beat 'em up**

- Move with WASD
- Attack with J/K/Space
- Combo chains
- Train through waves of enemies
- Face Zachariel at the end

---

## Controls

- `A/D` — Move left/right
- `W` — Jump
- `J` — Light attack (fast)
- `K` — Heavy attack (slow, more damage)
- `Space` — Special attack (uses meter)

---

## Combat System

### Combo Chains
- J J J = 3-hit combo
- J J K = knockdown
- K K = spin attack

### Training Enemies
| Enemy | HP | Behavior |
|-------|-----|----------|
| Training Dummy | 50 | Stands still, practice target |
| novice | 80 | Basic attacks |
| hunter | 120 | Can dodge |
| berserker | 200 | Aggressive, heavy hits |
| Sylva (boss) | 400 | Dual blades, fast |

---

## Wave Structure

1. **Wave 1-3** — Training dummies, learn combos
2. **Wave 4-6** — Novices, real combat begins
3. **Wave 7-9** — Hunters, they're faster now
4. **Wave 10** — Sylva fight
5. **Wave 11-13** — Berserkers, brutal
6. **Wave 14** — **Twist reveal** — Zachariel tells you the truth
7. **Wave 15** — Final fight

---

## The Twist

Wave 14 doesn't have enemies. Instead:

> **ZACHARIEL:** "You've done well, Callen. Better than I expected. But there's something you should know. The people you call your parents — the ones frozen in crystal — I was there that night. I led the attack. I was the one who gave the order."
>
> **ZACHARIEL:** "And Nathaniel? My brother? He tried to stop me. He failed. Now you train under the man who destroyed everything you loved. How does that feel?"

Wave 15 is the final fight — but now the player knows the truth.

---

## Visual Style

### Colors
```css
--bg-dark: #050508;
--ground: #0a0c10;
--callen: #4a6080;  /* Blue-grey, conflicted */
--zachariel: #804020; /* Warm orange-red, danger */
--enemy: #602020;
--combo: #c0a030;
--rage: #ff4040;
```

### Effects
- Screen shake on heavy hits
- Combo counter with glow
- Training montage style (speed lines between waves)
- Rage mode: red tint, faster attacks

---

## Game States

1. **Intro** — Backstory text, establish Callen
2. **Training** — Waves 1-3, learn controls
3. **Combat** — Real fighting begins
4. **The Twist** — Wave 14, revelation
5. **Final Stand** — Wave 15, fight Zachariel
6. **Ending** — What does Callen do now?

---

## Ending Options

- **Kill Zachariel** — Revenge, but at what cost?
- **Spare Zachariel** — Walk away. Choose who you become.

---

## Files

```
Games/
  callens-training/
    index.html
    PLAN.md
```
