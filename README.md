# Chronicles of Kalmirth — Lore Navigator

An immersive web-based lore exploration experience for the Chronicles of Kalmirth dark fantasy universe. This is not just a reference wiki—it's designed to make readers want to stay, explore, and return through discovery, investment, and atmosphere.

## 🌟 Features

### Core Experience
- **Interactive Lore Navigation** — Browse through 16+ pages of world-building content including characters, factions, timeline, geography, and magic systems
- **Progress Tracking** — Visual indicators show your reading progress per page and overall exploration completion
- **Lore Rank System** — Advance from Wanderer to Master of Kalmirth as you explore more content
- **Achievement System** — Unlock 13 achievements for completing milestones (First Steps, War Scholar, Art Collector, Full Chronicle, etc.)

### Immersive Elements
- **Scroll Animations** — Content reveals dynamically as you scroll, with special paragraph-by-paragraph reveals on Timeline and Plot pages
- **Ambient Atmospheres** — Page-specific particle effects (golden motes for Light Mages, dark wisps for Deepborn, crystal sparkles for Origins)
- **Cinematic Story Mode** — Toggle full-screen story presentation on Plot and Timeline pages
- **Spoiler/Revelation System** — Major plot revelations are hidden behind warnings and can be revealed at your own pace

### Interactive Features
- **Faction Allegiance Quiz** — 6-question quiz determines which of the 5 factions you belong to (Light, Nature, Druid, Dark, Fire, or Black Leaf)
- **Character Relationship Web** — Interactive node graph showing connections between all named characters
- **Quote Rotator** — "Heard in Kalmirth" displays rotating in-universe quotes on the home page
- **Reading Stats Panel** — Track total time spent, words read, pages explored, and achievements unlocked

### UX Polish
- **Estimated Reading Time** — Each page shows how long it takes to read
- **Continue Reading Footer** — Suggested next page at the bottom of each article
- **Lore Connection Chips** — "See Also" links connecting related topics
- **New/Updated Badges** — Recently modified content is highlighted
- **Hidden Easter Eggs** — Clickable secrets scattered throughout the lore

## 🏛️ The World of Kalmirth

### Factions
- **Light Mages** — "Light is life." Devotees of the sun and protection
- **Nature Mages** — Connected to forests, rivers, and natural balance
- **Druids** — Shapeshifters who believe in the divine possibility within all things
- **Dark Mages** — "When all light dies, the void remains." Philosophers of endings
- **Fire Mages** — "You prove yourself through fire." Warriors of transformation
- **Black Leaf Society** — Anti-magic extremists seeking to eliminate magic itself

### Key Characters
- **Nathaniel / Nate** — Protagonist with light powers, husband to Avery
- **Avery** — Last wiped Origin, adopted by Nathaniel
- **Zachariel** — Nathaniel's biological brother, complex antagonist
- **Faeron** — The last elven general, ancient warrior with survivor's grief
- **Fran** — Bonded with the entity Myst
- **Callen** — Biological father figure, killed Castor to repay a debt
- **Sanvi** — An Origin who brought magic to the world

### Major Lore Topics
- **The Great War** — Ancient conflict involving elves and the origin of current tensions
- **Origins** — Beings who first brought magic to Kalmirth
- **The Deepborn** — Ancient evil stirring below the world
- **Magic System** — How magic works, its sources, and costs
- **Geography & Locations** — Named places across the continent

## 🛠️ Technical Details

### Tech Stack
- **Vanilla HTML/CSS/JavaScript** — No build step required
- **jQuery 3.6.0** — DOM manipulation utilities
- **3D Force Graph** — Character relationship visualization
- **Page Flip Library** — Book-style page transitions
- **Google Fonts** — Cinzel (headings) and Open Sans (body text)
- **localStorage** — All user state persisted client-side (no login required)

### File Structure
```
/workspace
├── index.html              # Main Lore Navigator application
├── plan.md                 # Detailed upgrade roadmap
├── ART_PROMPTS.md          # Concept art generation prompts
├── GAME_FORMAT.md          # Game documentation format guide
├── agents.md               # Agent system documentation
├── page-flip.browser.js    # Page flip library
├── turn.min.js             # Turn.js library
├── Games/                  # Mini-games and interactive experiences
│   ├── index.html
│   ├── averys-children/
│   ├── blue-river/
│   ├── bronzefinger/
│   └── ... (more games)
├── Idle/                   # Idle game components
├── Character Concept/      # Character design files
├── Concept Art/            # Generated concept artwork
├── game-bg/                # Game background assets
└── night-origins/          # Night Origins specific content
```

### State Management
All user data stored in `localStorage` under key `kalmirth_state`:
```javascript
{
  visitedPages: { pageId: true },
  pageScrollDepth: { pageId: 0–100 },
  achievements: { id: timestamp },
  quizResult: { faction, score },
  lightboxOpened: { index: true },
  totalTimeMs: 0,
  visitCount: 0,
  lastVisit: timestamp,
  bookmarks: [{ pageId, selector, text }]
}
```

### Theme Support
- **Dark Theme (Default)** — Deep blues, golds, and warm parchment tones
- **Light Theme** — Cream, soft gold, and readable dark text
- **Prefers Reduced Motion** — Respects user accessibility preferences

## 🎮 Mini-Games

The `Games/` directory contains interactive experiences set in Kalmirth:

- **Avery's Children** — Narrative experience
- **Blue River** — Location-based adventure
- **Bronzefinger** — Character-focused story
- **Callen's Training** — Combat/skill development
- **Deepborn Siege** — Battle scenario
- **Labyrinth** — Puzzle/exploration game
- **Mythanar** — Expanded universe content
- **Night Origins** — Origin story exploration
- **Thea's Stand** — Heroic moment recreation
- **Vleyrra** — Elven storyline
- **Walking Sim** — Atmospheric exploration

## 📊 Development Roadmap

The project follows a phased implementation approach (see `plan.md`):

1. **Phase 1** — Foundation: State layer, scroll progress, sidebar dots ✅
2. **Phase 2** — Scroll animations and timeline/plot reveals ✅
3. **Phase 3** — Gamification: Ranks, achievements, achievement panel ✅
4. **Phase 4** — Signature features: Quiz, relationship web, story mode, spoilers
5. **Phase 5** — Reading polish: Reading time, continue footer, connection chips, atmospheres
6. **Phase 6** — Discovery extras: Stats panel, easter eggs

## 🎨 Art & Assets

- **Concept Art Gallery** — In-app lightbox viewing of generated artwork
- **ART_PROMPTS.md** — Detailed image generation prompts following established style anchor
- **Style Anchor** — Dark fantasy, painterly digital illustration in the style of Jakub Różalski, Jessica Rossier, Marc Simonetti

## 🚀 Getting Started

1. Open `index.html` in a modern web browser
2. No server or build process required
3. All features work offline after initial load
4. User progress persists automatically via localStorage

## 📖 Usage Tips

- **Explore systematically** — Check the sidebar progress dots to see what you've missed
- **Take the quiz early** — Your faction result will color your reading experience
- **Enable Story Mode** — For Timeline and Plot pages, toggle cinematic mode for maximum impact
- **Watch for reveals** — Spoiler cards contain major plot twists; reveal when ready
- **Check the relationship web** — Visual connections help understand character dynamics
- **Return visits** — The app tracks your history and shows new/updated content

## 🏆 Achievement List

| ID | Name | Trigger |
|---|---|---|
| `first_steps` | First Steps | Enter any page |
| `blood_magic` | Blood & Magic | Complete Characters page |
| `war_scholar` | War Scholar | Complete Great War page |
| `origin_seeker` | Origin Seeker | Complete Origins page |
| `faithful` | The Faithful | Complete Religion page |
| `deep_dread` | Deep Dread | Complete Deepborn page |
| `brotherhood` | Brotherhood | Complete Black Leaf page |
| `all_factions` | Alliance Forged | Complete all faction pages |
| `cartographer` | Cartographer | Complete Locations + Geography |
| `art_collector` | Art Collector | Open all 9 concept art images |
| `quiz_taken` | Faction Bound | Complete faction quiz |
| `full_chronicle` | Full Chronicle | Read all 16 pages (≥80% scroll) |
| `rank_up_master` | Master of Kalmirth | Reach highest rank |

## 🌐 Lore Rank Progression

| Pages Read | Rank |
|---|---|
| 0 | Wanderer |
| 1–3 | Apprentice |
| 4–6 | Scholar |
| 7–10 | Chronicler |
| 11–13 | Lorekeeper |
| 14–16 | Master of Kalmirth |

## 📝 Notes

- Total JavaScript: ~600–900 lines added to base HTML
- No external dependencies beyond listed libraries
- Fully client-side; no backend required
- Designed for desktop and mobile browsers
- Accessibility: respects reduced-motion preferences

## 🔮 Success Metrics

The project is considered successful when:
- First-time visitors spend 10+ minutes before leaving
- Users feel compelled to "complete" their progress bar
- Faction quiz results drive re-reading of faction pages
- The relationship web reveals previously unnoticed connections
- Story Mode feels like reading a graphic novel, not a reference doc

---

**Chronicles of Kalmirth** — A dark fantasy universe where light and shadow dance, brothers become enemies, and the last Origin holds the key to everything.

*"Why? Brother? Why'd you betray us!?"* — Zachariel
