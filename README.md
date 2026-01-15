# Chess Skills

A Vue.js prototype showcasing the **Skills** feature for chess game review.

## 🎯 What is this?

This project demonstrates the Skills UI feature - a gamification system that rewards players for recognizing chess patterns and tactics during game analysis. The prototype displays:

- **Game Review Interface** - Navigate through chess games move by move
- **Skill Recognition** - Highlights brilliant moves like sacrifices and tactics
- **Coach Feedback** - AI coach provides context and commentary on moves
- **Progress Tracking** - Visual progress bars and skill counters

## 🎮 Live Demo

**[View the prototype →](https://macakuaya.github.io/ChessSkills/)**

## 🏛️ Featured Game

The prototype loads **"The Immortal Game" (1851)** - Adolf Anderssen vs Lionel Kieseritzky, one of the most famous chess games ever played, featuring spectacular sacrifices including both rooks and the queen.

## 🛠️ Tech Stack

- **Vue 3** - Composition API with `<script setup>`
- **Vite** - Build tool and dev server
- **GitHub Pages** - Hosting

## 🚀 Getting Started

```bash
cd web
npm install
npm run dev
```

## 📁 Project Structure

```
web/
├── src/
│   ├── App.vue              # Main application
│   ├── components/          # UI components
│   │   ├── Board.vue        # Chess board
│   │   ├── CoachBubble.vue  # Coach feedback
│   │   ├── EvalBar.vue      # Position evaluation
│   │   └── MoveListBar.vue  # Move navigation
│   └── utils/
│       └── chess.js         # PGN parser & board logic
├── public/
│   ├── pieces/              # Chess piece images
│   ├── icons/               # UI icons
│   └── games/               # PGN files
└── index.html
```

## 📄 License

MIT
