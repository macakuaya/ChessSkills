# Skills Motion Prompt Plan (Vue)

This document turns the Figma motion specs into a prompt sequence for generating
Vue/Nuxt UI and interactions. Use the prompts in order to keep output consistent.

## Frame Inventory / Flow Map

- Skills · Motion Specs (container)
  - Title + Description (reference section)
  - Game review (multiple frames showing step-by-step state changes)
  - Move List Appears (move list fade-in)
  - Skills -> Bottom Sheet (bottom sheet overlay with skill info)
  - Animation Spec cards (timing/easing details)

## Shared Layout Regions

- Top: `V6 NavBar`
- Under nav: `Eval Bar`
- Coach area: coach avatar + bubble(s)
- Board area: 8x8 board with pieces and move markers (skewer/brilliant)
- Bottom: move list bar OR progress bar OR skill leave bar
- Tab bar / home indicator

## Reusable Components (with key props)

- `NavBar` (title, back, actions)
- `TabBar` (activeTab)
- `EvalBar` (evalText, blackWidth, whiteWidth)
- `CoachBubble` (messages[], variant: first/secondary, showTip)
- `Board` (squares, pieces[], highlights[], annotations[])
- `MoveMarker` (type: skewer|brilliant, label, position)
- `MoveListBar` (moves[], activeIndex, showIcons)
- `ProgressBar` (value, max, label, showExplosion)
- `SkillLeaveBar` (skillName, count, variant)
- `BottomSheet` (title, content, overlayVisible)
- `IconButton` (icon, onClick)
- `MoveClassificationBadge` (type)

## Motion Specs (from Animation Spec cards)

- Skill Progress Bar (slide-in)
  - animation: fade-in + move
  - duration: faster 100ms
  - curve: ease-out-strong (0, 0, 0.4, 1)
  - distance: +24px
  - direction: Vertical (y)
- Skill Progress Bar (slide-out)
  - animation: fade-out + move
  - duration: faster 100ms
  - curve: ease-out-strong (0, 0, 0.4, 1)
  - distance: -24px
  - direction: Vertical (y)
- Move List (fade-in)
  - duration: faster 100ms
  - curve: ease-out-strong (0, 0, 0.4, 1)
  - opacity: 0% -> 100%
- Move List (fade-out)
  - duration: faster 100ms
  - curve: ease-out-strong (0, 0, 0.4, 1)
  - opacity: 100% -> 0%
- Progress Bar Increment
  - animation: progress bar
- Counter Increment
  - animation: counter
- Explosion Starts
  - use current animation from board effects
- Square Celebration
  - use current animation from board effects
- Bottom Sheet
  - use current animation from motion library (bottom sheet + overlay)
- Piece Moves
  - standard board move animation (no explicit timing spec shown)

## Prompt Template (use for each prompt)

```
Goal: [what to build]
Context: Vue/Nuxt, mobile layout 375x812, reuse existing components if named.
Layout: [top/middle/bottom regions], spacing, alignment.
Components: [list], with props.
State: [state name], differences vs previous state.
Motion: [animation name], duration, easing, distance, direction, triggers.
Data: [mock data shape], defaults.
```

## Prompt Sequence (use in order)

1. **Base Tokens + Layout Shell**
   - Define the 375x812 mobile shell and layout regions (NavBar, Eval Bar,
     Coach area, Board, Bottom region, Tab bar).
   - Set typography and color tokens to match the Figma look.

2. **Shared Components**
   - Build `EvalBar`, `CoachBubble`, `Board`, `MoveListBar`, `ProgressBar`,
     `SkillLeaveBar`, `BottomSheet`, `MoveMarker`, `MoveClassificationBadge`.
   - Include props and empty/default states.

3. **Game Review Page (Base State)**
   - Render game review page with coach + board + move list bar.
   - Include example moves and a single move classification icon.

4. **Skill Point Earned Sequence**
   - Add state where skill point appears on a square (skewer/brilliant marker).
   - Trigger square celebration and explosion starts (board effects).
   - Show progress bar increment and counter increment.
   - Animate skill progress bar slide-in (+24px y, fade-in, 100ms, ease-out-strong).

5. **Skill Progress Bar Exit**
   - Animate skill progress bar slide-out (-24px y, fade-out, 100ms, ease-out-strong).
   - Replace bottom area with `SkillLeaveBar` state.

6. **Move List Appears**
   - Fade-in move list (100ms, ease-out-strong, 0% -> 100%).
   - If needed, fade-out old move list before new list appears.

7. **Skills Bottom Sheet**
   - Present bottom sheet overlay with skill summary and progress bar.
   - Use bottom sheet + overlay motion preset from motion library.

8. **Piece Move Animations**
   - Define default chess piece move animation for board transitions.

## Data Shapes (example)

- `Move`: { index: number, white: string, black?: string, classification?: string }
- `CoachMessage`: { id: string, text: string, variant: "first"|"secondary" }
- `Progress`: { value: number, max: number, label?: string }
- `Skill`: { id: string, name: string, count: number, icon: string }

## Coverage Notes

- Use the multiple `Game review` frames as a state timeline (base → skill earned
  → explosion → progress bar increment → skill leave).
- `Move List Appears` is its own transition state.
- `Skills -> Bottom Sheet` is a separate overlay flow.
