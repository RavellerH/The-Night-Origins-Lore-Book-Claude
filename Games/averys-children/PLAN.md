# Avery's Children — Co-op Platformer Plan

> Play as Lucas, Sheriel, and Afriel. Three siblings. Three crystal powers. One adventure.

---

## Lore Context

Many years after Book 1. Avery and Nate have three children:
- **Lucas** (blue crystal) — eldest, protective
- **Sheriel** (purple crystal) — middle, curious
- **Afriel** (green crystal) — youngest, fearless

They're exploring the ruins near the Alliance base. Nothing dangerous. Probably. Maybe.

---

## Game Concept

**Side-scrolling co-op platformer**

- Control 3 characters with different abilities
- Switch between characters with 1/2/3 keys
- Each character has a unique crystal power
- Solve puzzles together to reach the exit
- Lighter tone, kids adventure

---

## Characters

| Character | Color | Ability |
|-----------|-------|---------|
| Lucas | Blue 💎 | Can freeze water/pits to create platforms |
| Sheriel | Purple 💎 | Can create crystal bridges across gaps |
| Afriel | Green 💎 | Can climb walls and vines |

---

## Controls

- `A/D` — Move left/right
- `W` — Jump
- `1` — Switch to Lucas
- `2` — Switch to Sheriel
- `3` — Switch to Afriel
- `Space` — Use ability (context-sensitive)
- `E` — Interact with objects

---

## Level Design

### Simple puzzle example:
- Gap too wide to jump
- Switch to Lucas, freeze the water below into a platform
- All three cross
- Next obstacle: need to reach high ledge
- Switch to Afriel, climb the wall
- Activate switch to lower bridge for others

---

## Game Structure

1. **Title** — Introduction to the three kids
2. **Level 1** — Tutorial: basic movement, switch characters
3. **Level 2** — Introduce Lucas's freeze ability
4. **Level 3** — Introduce Sheriel's bridge ability
5. **Level 4** — Introduce Afriel's climb ability
6. **Level 5** — Full puzzle combining all three

---

## Visual Style

```css
--bg-sky: #1a2a3a;
--ground: #2a3a4a;
--lucas: #4a9ad0;    /* Blue crystal */
--sheriel: #8060a0;  /* Purple crystal */
--afriel: #50a050;   /* Green crystal */
--crystal-glow: rgba(100, 180, 255, 0.5);
```

---

## Features

- [ ] 3 playable characters with switching
- [ ] Platform physics (gravity, collision)
- [ ] Unique abilities per character
- [ ] Level with cooperative puzzle
- [ ] Character indicators (which is active)
- [ ] Ability indicators
- [ ] Victory condition (reach end)

---

## Files

```
Games/
  averys-children/
    index.html
    PLAN.md
```
