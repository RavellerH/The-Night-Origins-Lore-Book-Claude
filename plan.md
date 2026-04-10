# Lore Navigator — Immersive Upgrade Plan

## Goal
Transform the webapp from a reference tool into an experience that makes readers want to stay, explore, and return. Every feature should serve one of three drives: **discovery**, **investment**, or **atmosphere**.

---

## Phase 1 — Foundation: State & Progress Layer
*Everything else depends on this. Build first.*

### 1.1 localStorage State System
Track all user activity across sessions. No login needed — all client-side.

```js
State shape:
{
  visitedPages: { pageId: true },        // pages ever opened
  pageScrollDepth: { pageId: 0–100 },    // max % scrolled per page
  achievements: { id: timestamp },       // unlocked achievements
  quizResult: { faction, score },        // faction quiz result
  lightboxOpened: { index: true },       // concept art images opened
  totalTimeMs: 0,                        // cumulative time on site
  visitCount: 0,                         // number of sessions
  lastVisit: timestamp,
  bookmarks: [{ pageId, selector, text }] // saved highlights
}
```

### 1.2 Per-Page Scroll Progress Bar
- Thin gold line at the very top of `#content-wrap`
- Fills as you scroll down the current page (0–100%)
- Resets visually when navigating to a new page
- Updates `pageScrollDepth` in state when > previous max

### 1.3 Global Explorer Progress
- In sidebar header: circular arc progress indicator
- Tracks pages where `pageScrollDepth >= 80%` as "read"
- Shows: `8 / 16 pages explored`
- Rank label beneath (see Section 3)

### 1.4 Sidebar Read Status Dots
Each nav item gets a small status indicator to the right:
- No dot — never opened
- Small dim dot — opened, < 30% scrolled
- Half-filled dot — 30–79% scrolled
- Gold filled dot — ≥ 80% scrolled (complete)

---

## Phase 2 — Scroll Animations
*Makes the content feel alive instead of static.*

### 2.1 Entry Animations (IntersectionObserver)
Every `.char-card`, `.faction-card`, `.parchment`, `.lore-table`, `.note` starts invisible and animates in when it enters the viewport.

Animation: `opacity 0 → 1` + `translateY(16px) → 0`, duration 0.4s, ease-out.

Cards stagger: each sibling delays by 60ms more than the previous.

### 2.2 Timeline / Plot Paragraph-by-Paragraph Reveal
On the **Timeline** and **Plot** pages specifically:
- Each `.timeline-entry` and each plot paragraph (`<p>` inside `.plot-beat`) is treated as an individual reveal unit
- They appear one by one as you scroll, not all at once
- Creates a storytelling "unfolding" feeling — events emerge as you move through history
- The timeline dot animates separately (scale 0 → 1 with a glow pulse)

### 2.3 Section Title Underline Draw
When a `.subtitle` enters the viewport, its left border "grows" from 0 height to full height. Small detail, high polish.

---

## Phase 3 — Gamification
*Gives readers a reason to explore beyond what they came for.*

### 3.1 Lore Rank System
Shown in sidebar header beneath the progress arc. Based on % of pages fully read (≥ 80% scroll depth).

| Pages Read | Rank | Color |
|---|---|---|
| 0 | Wanderer | dim gray |
| 1–3 | Apprentice | silver |
| 4–6 | Scholar | soft gold |
| 7–10 | Chronicler | gold |
| 11–13 | Lorekeeper | bright gold |
| 14–16 | Master of Kalmirth | gold + glow |

Rank upgrades trigger an achievement toast.

### 3.2 Achievement System
Achievements unlock as the user explores. Each shows as a toast notification (bottom-right, slides in, 4 seconds, then fades).

Toast design: dark card, gold left border, achievement icon, name, description.

| ID | Name | Description | Trigger |
|---|---|---|---|
| `first_steps` | First Steps | *You have entered Kalmirth.* | Any page opened |
| `blood_magic` | Blood & Magic | *The characters of this world are known to you.* | Characters page ≥ 80% |
| `war_scholar` | War Scholar | *You know what the elves did.* | Great War ≥ 80% |
| `origin_seeker` | Origin Seeker | *The portal holds no secrets from you.* | Origins ≥ 80% |
| `faithful` | The Faithful | *You understand what the factions believe.* | Religion ≥ 80% |
| `deep_dread` | Deep Dread | *You know what stirs below.* | Deepborn ≥ 80% |
| `brotherhood` | Brotherhood | *You know the truth about the brothers.* | Black Leaf ≥ 80% |
| `all_factions` | Alliance Forged | *All five factions are known to you.* | Factions ≥ 80% |
| `cartographer` | Cartographer | *Every named place is mapped in your mind.* | Locations + Geography ≥ 80% each |
| `art_collector` | Art Collector | *You have seen the world rendered.* | All 9 concept art images opened |
| `quiz_taken` | Faction Bound | *You know where you belong.* | Faction quiz completed |
| `full_chronicle` | Full Chronicle | *The complete history of Kalmirth lives in you.* | All 16 pages ≥ 80% |
| `rank_up_master` | Master of Kalmirth | *There is nothing left to learn. Only to live it.* | Rank reaches Master |

### 3.3 Achievement Collection Panel
A page accessible from the sidebar (`🏆 Achievements`) showing all achievements in a grid:
- Locked achievements shown as dim silhouettes with `???` name
- Unlocked show the icon, name, description, and unlock date
- Progress counters: `7 / 13 unlocked`

---

## Phase 4 — Signature Features

### 4.1 Faction Allegiance Quiz
Accessible from home page tile and its own nav item (`⚔️ Which Faction?`).

**Format:** 6 questions, each with 5 answers (one per faction). No timer. Clean full-card question layout.

**Sample questions:**
1. *When a conflict arises, your first instinct is to...* → (Light: protect someone, Nature: listen before acting, Druid: adapt your approach, Dark: wait and see, Fire: act now)
2. *What do you believe is the most honest thing about the world?*
3. *Which sacrifice would you make willingly?*
4. *Where would you rather spend a night?*
5. *What do you think about magic?*
6. *What does victory mean to you?*

**Result screen:** Full-faction styled card with faction color header, faction name, theological quote, description of what the result means about you, and a "Read their story →" button linking to the Factions page.

Result saved to state, shown in sidebar as a small faction badge.

### 4.2 Character Relationship Web
A dedicated page (`👥 Relationships`) with an interactive SVG/Canvas node graph.

**Nodes:** One per named character, sized by story importance.
**Edges:** Lines with labels — *brother, father, mentor, enemy, bond, saved, ward*.
**Colors:** Nodes colored by faction affiliation.

**Interaction:**
- Hover a node → shows character name + role tooltip
- Click a node → navigates to Characters page (scrolled to that character)
- Drag to pan, scroll to zoom

**Connections to map:**
- Nathaniel ↔ Zachariel (biological brothers)
- Faeron → Ryllae (father, left without a word)
- Ryllae → Avery (adoptive mother, found her)
- Nathaniel → Avery (adoptive father)
- Nathaniel → Callen (biological father)
- Zachariel → Callen (uncle/ward)
- Nate → Avery (husband)
- Sanvi → Nate (mother)
- Nathaniel → Sanvi (saved her)
- Fran → Myst (Bond)
- Phaerille → Fran (designed the Bond)
- Silas → Callen (best friend, died)
- Castor → Silas (killed)
- Callen → Castor (killed, debt repaid)
- Zachariel → Castor (indirect, same war)

### 4.3 Cinematic Story Mode
A toggle on the Plot and Timeline pages: `[ ] Story Mode`.

When enabled:
- Full-screen dark overlay takes over the content area
- Each plot beat / timeline event becomes a "slide"
- Large centered text, minimal UI, very cinematic
- Prev/Next navigation or scroll-to-advance
- Progress dots at the bottom showing position in the sequence
- Escape to exit Story Mode

Feels like reading a graphic novel or watching a title card sequence.

### 4.4 Spoiler / Revelation System
Certain lore cards are marked as major revelations and hidden by default.

**Hidden content:**
- The Elven Secret (Great War page)
- Faeron's true identity as Ryllae's father
- Nathaniel and Zachariel being biological brothers
- The memory shift — Avery is the last wiped Origin
- What ends the story (Avery seals herself in crystal)

**UI:** Blurred/darkened card with:
> ⚠ *This is a significant revelation. Reading this will change how you see everything before it.*
> `[ Reveal ]` button

Clicking reveals with a flash/unfurl animation.

### 4.5 "Heard in Kalmirth" — Quote Rotator
On the home page, below the grid: a rotating quote display cycling through 10–12 in-universe lines every 6 seconds with a crossfade.

**Quotes to include:**
- *"Why? Brother? Why'd you betray us!?"* — Zachariel
- *"Light is life. Imagine every sun that exists — there is life there."* — Nate
- *"It was Origin who brought magic to this world for the first time... Yes, you're not from here, Avery."* — Sanvi
- *"The answer you're looking for is inside yourself. You already know it. Trust it."* — Faeron
- *"My god is not the sun. He rules all suns."* — Nate
- *"When all light dies, the void remains."* — Dark Mage doctrine
- *"You prove yourself through fire."* — Fire Mage doctrine
- *"The forest is conscious. The river remembers. The mountain watches."* — Nature Mage doctrine
- *"Before there was a wolf or a man, there was the possibility of both. That possibility is divine."* — Druid doctrine
- *"Magic is not divine. It is a force. It can be eliminated."* — Black Leaf doctrine

---

## Phase 5 — Reading Experience Polish

### 5.1 Estimated Reading Time
Small badge on each page tile (home grid) and next to the page title in the topbar:
`~4 min read` — calculated from word count of the render() output.

### 5.2 "Continue Reading" Footer
At the bottom of every page: a suggested next page with title, description, and arrow. Sequence follows a logical reading order (Plot → Timeline → Characters → Factions → ...). Clicking it navigates there with the page-enter animation.

### 5.3 Lore Connections Chips
At the bottom of every parchment card, before the Continue footer: a `See Also →` row of clickable chips linking to related pages. Hand-mapped per page.

```
Characters → See Also: Factions, Relationships, Origins
Great War  → See Also: Deepborn, Elves (Religion), Faeron (Characters)
Origins    → See Also: Characters (Fran, Avery), Magic, Religion
```

### 5.4 Ambient Page Atmosphere
Subtle background particle systems that vary per page category. Uses canvas, very low CPU cost.

| Page | Atmosphere |
|---|---|
| Light Mages / Religion | Slow drifting golden motes |
| Deepborn / Great War | Faint dark smoke wisps |
| Origins / Magic | Faint crystal-blue sparkle drift |
| Characters | Neutral (none) |
| Concept Art | None |

### 5.5 "New / Updated" Badges
Each LORE entry gets a `lastUpdated: 'YYYY-MM-DD'` field. If the date is within 14 days, the nav item and home tile show a small pulsing `NEW` or `UPDATED` badge.

---

## Phase 6 — Discovery Extras

### 6.1 Reading Stats Panel
A collapsible panel in the sidebar footer (or its own page) showing:
- Total pages explored / 16
- Estimated total words read
- Time spent in Kalmirth (this session + total)
- Faction badge (from quiz)
- Achievements unlocked / 13

### 6.2 Hidden Lore Fragments (Easter Eggs)
5–6 small hidden interactions scattered in the content:
- Clicking the ⚜ crest 3 times plays a subtle chime and shows a hidden quote
- A specific word in the lore (e.g., "void") — clicking it reveals a hidden Dark Mage fragment
- The Blue River glow description has a hidden tooltip: *"You felt it too, didn't you?"*

---

## Implementation Order

```
Phase 1 — State layer + scroll progress + sidebar dots     (foundation)
Phase 2 — Scroll reveal animations + timeline/plot reveal  (biggest visual impact)
Phase 3 — Rank + Achievements + Achievement panel          (gamification core)
Phase 4.5 — Quote rotator on home                          (quick win, home page impact)
Phase 4.4 — Spoiler/revelation system                      (high drama value)
Phase 5.2 — Continue Reading footer                        (keeps users flowing)
Phase 5.1 — Reading time estimates                         (small, quick)
Phase 5.3 — Lore connection chips                          (adds web of discovery)
Phase 4.3 — Cinematic Story Mode                           (big feature, Timeline/Plot)
Phase 4.1 — Faction Allegiance Quiz                        (sharable, fun)
Phase 4.2 — Character Relationship Web                     (visual, complex)
Phase 5.4 — Ambient atmospheres                            (polish)
Phase 6.1 — Reading Stats panel                            (nice-to-have)
Phase 6.2 — Easter eggs                                    (bonus)
```

---

## Technical Notes

- All state in `localStorage` key `kalmirth_state` — one object, serialized as JSON
- No external dependencies added except what's already there (Google Fonts)
- Canvas used for ambient particles (optional, toggleable)
- Relationship graph built with plain Canvas API — no D3 or external library
- Quiz, achievements, stats all self-contained in the `<script>` block
- Total estimated JS addition: ~600–900 lines
- No build step required — everything added to `index.html`

---

## Success Metrics (what "working" looks like)

- A first-time visitor spends 10+ minutes before leaving
- The sidebar shows clear visual progress that makes them want to "complete" the pages
- The faction quiz result makes them go back and read that faction's page
- The relationship web makes them realize connections they hadn't noticed
- Story Mode on the Timeline feels like reading the story, not a reference doc
