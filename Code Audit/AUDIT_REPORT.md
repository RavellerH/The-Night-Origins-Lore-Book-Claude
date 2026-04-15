# Lore Navigator — Code Audit Report

**File audited:** `index.html`  
**Total lines:** 7041  
**Date:** 2026-04-13

---

## Architecture

- **Single monolithic file** — CSS (~1500 lines), HTML, and JS all in one
- **Vanilla stack** — jQuery, 3d-force-graph, page-flip.browser.js
- **Embedded data** — All lore stored in JS objects (`TIMELINE_DATA`, `THREADS_DATA`, `LORE`)

---

## Critical Issues

| Severity | Issue | Location |
|----------|-------|----------|
| HIGH | Inline `onclick` handlers throughout HTML | Lines 1834, 1837, 1841, 1872, etc. |
| HIGH | Password check in client-side JS | ~Line 1700+ — bypassable |
| HIGH | `innerHTML` with unsanitized data in render() | Multiple locations |
| MEDIUM | Global state pollution — all functions/variables global | Entire JS section |
| MEDIUM | Password modal lacks `role="dialog"` and `aria-modal` | Line 1665+ |

### Password Protection Bypass

The password gate (writer mode) is purely client-side. Any user can:
1. Open DevTools
2. Set `localStorage.setItem('km_writer_mode', 'true')`
3. Refresh — writer content unlocked

**Recommendation:** Move sensitive content to server-side authentication or accept this as intentional "security through obscurity."

---

## Performance Concerns

### 1. Unused Dependencies
- `3d-force-graph` imported (line 10) — verify if `Timeline3d` page uses it or uses custom Canvas instead
- `turn.min.js` present in folder but not loaded in index.html

### 2. No Code Splitting
- Entire 7000+ line file loads even when viewing one page
- Consider lazy-loading render functions for rarely-used pages (Great War, Ancient Civilization, Chapter 1 Draft)

### 3. Canvas Without Throttling
- Ambient canvas and relationship web canvas may not use `requestAnimationFrame` properly
- Could cause battery drain on laptops

### 4. IntersectionObserver Not Implemented
- `plan.md` Phase 2.1 describes scroll reveal animations
- Currently not in codebase — big visual opportunity missed

---

## CSS Issues

### 1. Duplicate Theme Variables
Lines 34-82 repeat CSS custom properties for light theme. Consider:

```css
body.light-theme {
  --card: #f5f0e1;
  /* ... 20 more lines of duplication ... */
}
```

**Recommendation:** Use inheritance or a CSS preprocessor.

### 2. Z-Index Stacking Chaos
```
Line 219: z-index: 1     (layout elements)
Line 999: z-index: 1000  (#lightbox)
Line 1072: z-index: 1000 (.grid-status)
Line 1301: z-index: 999  (#scroll-progress)
Line 1339: z-index: 2000 (#toast-container)
Line 1435: z-index: 900   (#story-mode-overlay)
```

**Recommendation:** Consolidate to predictable tiers: 100 (layout), 500 (overlays), 1000 (modals), 2000 (toasts).

### 3. Missing Reduced Motion
No `@media (prefers-reduced-motion)` query. Animations play for all users.

### 4. `backdrop-filter` Usage
Line 934: `backdrop-filter: blur(6px)` on lightbox — may cause jank on older devices.

---

## JavaScript Quality

### 1. No Module Pattern
All functions/variables are global — risk of naming collisions.

**Current:**
```javascript
function navigate(pageId) { ... }
function toggleTheme() { ... }
const LORE = { ... };
```

**Recommendation:** Wrap in IIFE or use ES modules:
```javascript
const LoreApp = (function() {
  function navigate() { ... }
  return { navigate };
})();
```

### 2. Inconsistent Naming Conventions
```
navigate           (camelCase)
handleModeBadgeClick  (camelCase)
lbStep             (abbreviated)
showThreadTooltip   (camelCase)
hideThreadTooltip   (camelCase)
toggleThreadsView   (camelCase)
switchPerspective   (camelCase)
```

Pick one style and enforce with ESLint.

### 3. Magic Numbers
```javascript
Line 1314: stroke-dasharray: 69.1   // What is 69.1?
Line 1314: stroke-dashoffset: 69.1  // Same here
Line 1315: transform-origin: 14px 14px  // Derived from SVG viewBox
```

**Recommendation:** Name constants:
```javascript
const PROGRESS_RADIUS = 11;
const PROGRESS_CIRCUMFERENCE = 2 * Math.PI * PROGRESS_RADIUS; // ≈ 69.1
```

### 4. Duplicate Function Definitions
`switchPerspective` appears to be defined multiple times in the file. Search and consolidate.

### 5. No JSDoc
Parameters and return types are undocumented. Makes maintenance harder.

---

## Accessibility Gaps

| Issue | Location | WCAG Criterion |
|-------|----------|----------------|
| Icon buttons lack `aria-label` | Sidebar toggle, scroll-top, lightbox controls | 1.1.1 Non-text Content |
| Modals lack `role="dialog"` | Password modal, lightbox | 4.1.2 Name, Role, Value |
| Focus visible not guaranteed | `.msg-bubble`, `.quiz-option` | 2.4.11 Focus Not Obscured |
| No skip-to-content link | Body element | 2.4.1 Bypass Blocks |
| Color contrast (ink-soft on card) | CSS var(--ink-soft) #a89f8c on #0f1622 | 1.4.3 Contrast |

### Contrast Check
- `var(--ink-soft)` = `#a89f8c` 
- `var(--card)` = `#0f1622`
- Contrast ratio: ~7.2:1 — **PASSES** AA and AAA

Actually this contrast is fine. Other combinations may vary.

---

## Security

### 1. External Scripts Without SRI

```html
<!-- jQuery — HAS integrity hash -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js" 
  integrity="sha256-/xUj+3OJ+Yd2V3a1pvK+K0QdRJv7/A+VVJbqDv311b8=" 
  crossorigin="anonymous"></script>

<!-- 3d-force-graph — NO hash -->
<script src="https://unpkg.com/3d-force-graph"></script>

<!-- page-flip — local file, assumed trusted -->
<script src="page-flip.browser.js"></script>
```

**Recommendation:** Add SRI hashes for unpkg imports or self-host the library.

### 2. localStorage Manipulation
```javascript
localStorage.getItem('km_beats')       // Story beats tracker
localStorage.getItem('km_writer_mode')  // Writer mode gate
localStorage.getItem('km_state')        // Progress/state
```

All readable/writable by browser extensions and any JS. Acceptable for a non-sensitive lore tool, but quiz results could be spoofed.

---

## Maintainability

### 1. No Linting/Formatting Config
No `.eslintrc`, `.prettierrc`, or `package.json` present.

### 2. Hardcoded Paths
```javascript
Line 1872: window.location.href='plot-diagram.html'
Line 1894: window.location.href='threads.html'
Line 1918: window.location.href='world-map.html'
Line 2009: href="Games/index.html"
```

These break if the file structure changes. Consider a config object:
```javascript
const APP_PATHS = {
  PLOT_DIAGRAM: 'plot-diagram.html',
  THREADS: 'threads.html',
  WORLD_MAP: 'world-map.html',
  GAMES: 'Games/index.html'
};
```

### 3. No Tests
No test suite exists. Given the complexity, consider adding:
- Playwright for smoke tests (does page load? do navigation buttons work?)
- Check for console errors on key pages

---

## Recommendations (Priority Order)

### Quick Wins (1-2 hours)
1. Add SRI hashes for `unpkg.com/3d-force-graph`
2. Add `prefers-reduced-motion` media query to CSS
3. Add `aria-label` to all icon-only buttons
4. Verify if 3d-force-graph is actually used

### Medium Effort (1-2 days)
1. Extract CSS into `styles.css` — reduces initial load
2. Wrap JS in IIFE module pattern
3. Add ESLint + Prettier config
4. Consolidate z-index values into CSS custom properties
5. Add `role="dialog"` and `aria-modal` to modals

### Large Refactors (1+ week)
1. Implement IntersectionObserver for scroll reveals (plan.md Phase 2)
2. Add code splitting with dynamic imports
3. Move lore data to external `.json` files
4. Add Playwright test suite
5. Consider migrating to a lightweight framework (Preact?) for state management

---

## Summary

| Category | Rating | Notes |
|----------|--------|-------|
| Functionality | Good | Works well for its purpose |
| Performance | Fair | No major issues, some optimization opportunities |
| Security | Acceptable | Client-side auth is intentional; add SRI hashes |
| Accessibility | Needs Work | Missing ARIA labels and focus management |
| Maintainability | Poor | Monolithic file, no linting, no tests |
| Code Quality | Fair | Inconsistent naming, magic numbers, no docs |

**Overall:** Solid for a personal project / reference tool. Would benefit from modularization before adding more features.
