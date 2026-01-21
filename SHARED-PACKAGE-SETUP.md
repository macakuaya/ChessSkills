# Shared Animation Package Setup

A guide to extract reusable animations from ChessSkills into a shared npm package.

---

## Phase 1: Create the shared package repo

- [ ] **1.1** Create a new GitHub repo called `chess-animations` (or similar)
- [ ] **1.2** Clone it locally
- [ ] **1.3** Set up the package structure:
  ```
  chess-animations/
  ├── src/
  │   ├── board-animations/
  │   │   ├── SkillHighlight.vue
  │   │   ├── BrilliantHighlight.vue
  │   │   └── ExplosionEffect.vue
  │   ├── celebrations/
  │   │   ├── BoardCelebration.vue
  │   │   ├── SkillEarned.vue
  │   │   └── SkillUnlockedModal.vue
  │   └── sounds/
  │       └── useSound.js
  ├── package.json
  └── README.md
  ```
- [ ] **1.4** Extract animation code from ChessSkills into the new package
- [ ] **1.5** Push to GitHub

---

## Phase 2: Update ChessSkills to use the package

- [ ] **2.1** Install the package:
  ```bash
  npm install github:macakuaya/chess-animations
  ```
- [ ] **2.2** Update imports in `App.vue` and other files:
  ```javascript
  // Before
  import BoardCelebration from './components/BoardCelebration.vue'
  
  // After
  import { BoardCelebration } from '@chess/animations'
  ```
- [ ] **2.3** Delete the old animation component files (now in the package)
- [ ] **2.4** Test everything still works
- [ ] **2.5** Commit and push

---

## Phase 3: For each new prototype

- [ ] **3.1** Create new GitHub repo
- [ ] **3.2** Set up Vue project:
  ```bash
  npm create vue@latest
  ```
- [ ] **3.3** Install the animations package:
  ```bash
  npm install github:macakuaya/chess-animations
  ```
- [ ] **3.4** Import and use the components
- [ ] **3.5** Set up GitHub Pages deployment
- [ ] **3.6** Deploy

---

## Maintenance: When you update animations later

1. Make changes in `chess-animations` repo
2. Push to GitHub
3. In each prototype that needs the update:
   ```bash
   npm update @chess/animations
   ```

---

## Package contents reference

### board-animations/
| Component | Description |
|-----------|-------------|
| `SkillHighlight` | Gold overlay + star icon + label bubble → coin fall |
| `BrilliantHighlight` | Teal overlay + exclamation icon + label |
| `ExplosionEffect` | Semicircle burst at board edge |

### celebrations/
| Component | Description |
|-----------|-------------|
| `BoardCelebration` | Full overlay with image, title, subtitle |
| `SkillEarned` | Progress bar with animated slot-machine counter |
| `SkillUnlockedModal` | Hero modal for skill mastery |

### sounds/
| Export | Description |
|--------|-------------|
| `useSound()` | Composable to play chess sounds from CDN |

---

## Notes

- Package will be installed via GitHub (no npm publish needed)
- Each prototype gets its own repo and GitHub Pages URL
- Animations package is shared across all prototypes
