<script setup>
import { ref, computed, onMounted } from 'vue'
import EvalBar from './components/EvalBar.vue'
import CoachBubble from './components/CoachBubble.vue'
import Board from './components/Board.vue'
import MoveListBar from './components/MoveListBar.vue'
import ProgressBar from './components/ProgressBar.vue'
import SkillLeaveBar from './components/SkillLeaveBar.vue'
import BottomSheet from './components/BottomSheet.vue'
import MoveMarker from './components/MoveMarker.vue'
import MoveClassificationBadge from './components/MoveClassificationBadge.vue'
import { parsePGN, calculatePositions, boardToPieces, markBrilliantMoves } from './utils/chess.js'

const showComponents = ref(false)
const activePly = ref(0)

// Game data
const gameData = ref(null)
const positions = ref([])
const moveList = ref([])

// Computed pieces based on current ply
const pieces = computed(() => {
  if (!positions.value.length || activePly.value < 0) return []
  const posIndex = Math.min(activePly.value, positions.value.length - 1)
  console.log(`Showing position ${posIndex} of ${positions.value.length} (activePly: ${activePly.value})`)
  return boardToPieces(positions.value[posIndex])
})

// Load PGN file
async function loadGame(pgnPath) {
  try {
    const response = await fetch(pgnPath)
    const pgnText = await response.text()
    
    // Parse PGN
    gameData.value = parsePGN(pgnText)
    
    // Mark brilliant moves (for The Immortal Game)
    // 18. Bd6 (rook sac #1), 19. e5 (rook sac #2), 22. Qf6+ (queen sac), 23. Be7# (checkmate)
    const brilliantMoves = [18, 19, 22, 23]
    moveList.value = markBrilliantMoves(gameData.value.moves, brilliantMoves)
    
    // Calculate all positions
    positions.value = calculatePositions(gameData.value.moves)
    
    // Start at position 0 (initial position)
    activePly.value = 0
  } catch (error) {
    console.error('Failed to load PGN:', error)
  }
}

// Load The Immortal Game on mount
onMounted(() => {
  loadGame('/games/immortal-game-1851.pgn')
})

const evalText = 'M7'

const coachMessages = [
  { id: 'm1', text: 'Nice find! That move wins material.' },
  { id: 'm2', text: 'Now convert carefully.' },
]

const skillProgress = {
  name: 'Skewer',
  value: 40,
  max: 100,
  counter: 10,
}

const navIcons = {
  back: '/icons/arrow-back.svg',
  search: '/icons/search.svg',
  settings: '/icons/settings.svg',
}
</script>

<template>
  <div class="app">
    <div class="phone-frame">
      <header class="nav-bar">
        <div class="status-bar">
          <div class="status-right">
            <img alt="" src="/icons/status-right.svg" />
          </div>
        </div>
        <div class="nav-header">
          <div class="nav-icon-button">
            <img alt="Back" :src="navIcons.back" />
          </div>
          <div class="nav-title">Game Review</div>
          <div class="nav-actions">
            <div class="nav-icon-button">
              <img alt="Search" :src="navIcons.search" />
            </div>
            <div class="nav-icon-button">
              <img alt="Settings" :src="navIcons.settings" />
            </div>
          </div>
        </div>
      </header>

      <section class="eval-section">
        <EvalBar :eval-text="evalText" :black-width="92" :white-width="5" />
      </section>

      <section class="coach-area">
        <CoachBubble 
          header-icon="/icons/brilliant.svg"
          header-text="The Immortal Game (1851)"
          eval-text=""
          message="Anderssen sacrificed a bishop, both rooks, and his queen to deliver checkmate!"
          :show-tip="true"
        />
      </section>

      <section class="board-area">
        <Board :pieces="pieces" :size="375" />
      </section>

      <section class="content-area">
        <MoveListBar
          :moves="moveList"
          v-model:active-ply="activePly"
          :show-icons="true"
        />
      </section>

      <footer class="tab-bar">
        <div class="tabs-container">
          <div class="tab-item">
            <div class="tab-icon">
              <img src="/icons/tab-skills.svg" alt="" />
            </div>
            <span class="tab-label">Skills</span>
          </div>
          <div class="tab-item">
            <div class="tab-icon">
              <img src="/icons/tab-show.svg" alt="" />
            </div>
            <span class="tab-label">Show</span>
          </div>
          <div class="tab-item">
            <div class="tab-icon">
              <img src="/icons/tab-best.svg" alt="" />
            </div>
            <span class="tab-label">Best</span>
          </div>
          <button class="tab-cta">
            <span>Next</span>
          </button>
        </div>
        <div class="home-indicator"></div>
      </footer>
    </div>

    <!-- Toggle button for components -->
    <button class="toggle-components" @click="showComponents = !showComponents">
      {{ showComponents ? '✕ Hide' : '☰ Components' }}
    </button>

    <section v-if="showComponents" class="components-showcase">
      <h2>Shared Components</h2>

      <div class="component-card">
        <h3>EvalBar</h3>
        <EvalBar :eval-text="'M7'" :black-width="92" :white-width="8" />
        <EvalBar />
      </div>

      <div class="component-card">
        <h3>CoachBubble</h3>
        <CoachBubble :messages="coachMessages" variant="first" :show-tip="true" />
        <CoachBubble :messages="[]" variant="secondary" :show-tip="false" />
      </div>

      <div class="component-card">
        <h3>Board</h3>
        <Board :pieces="pieces" :highlights="['e4', 'f6']" :annotations="['skewer']" />
        <Board :pieces="[]" />
      </div>

      <div class="component-card">
        <h3>MoveListBar</h3>
        <MoveListBar :moves="moveList" :active-ply="0" :show-icons="true" />
        <MoveListBar />
      </div>

      <div class="component-card">
        <h3>ProgressBar</h3>
        <ProgressBar :value="50" :max="100" label="Skill Progress" :show-explosion="true" />
        <ProgressBar />
      </div>

      <div class="component-card">
        <h3>SkillLeaveBar</h3>
        <SkillLeaveBar skill-name="Fork" :count="10" />
        <SkillLeaveBar />
      </div>

      <div class="component-card">
        <h3>BottomSheet</h3>
        <BottomSheet title="Skill Details" content="Your fork skill improved." :overlay-visible="true" />
        <BottomSheet />
      </div>

      <div class="component-card">
        <h3>MoveMarker</h3>
        <MoveMarker type="skewer" label="Skewer" position="e4" />
        <MoveMarker type="brilliant" label="Brilliant" position="f6" />
      </div>

      <div class="component-card">
        <h3>MoveClassificationBadge</h3>
        <MoveClassificationBadge type="brilliant" />
        <MoveClassificationBadge type="blunder" />
        <MoveClassificationBadge />
      </div>
    </section>
  </div>
</template>

<style>
/* Chess Sans Variable Font */
@font-face {
  font-family: 'Chess Sans';
  src: url('/fonts/ChessSansVF.ttf') format('truetype');
  font-weight: 100 900;
  font-style: normal;
  font-display: swap;
}
</style>

<style scoped>
.app {
  background: #1f1f1f;
  min-height: 100vh;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  gap: 32px;
  padding: 24px;
  font-family: 'Chess Sans', system-ui, -apple-system, sans-serif;
}

.toggle-components {
  position: fixed;
  top: 16px;
  right: 16px;
  background: #81b64c;
  color: #fff;
  border: none;
  padding: 8px 16px;
  border-radius: 8px;
  font-family: inherit;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  z-index: 1000;
  transition: background 0.2s;
}

.toggle-components:hover {
  background: #6a9a3f;
}

.phone-frame {
  width: 375px;
  height: 812px;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0.3) 0%, rgba(0, 0, 0, 0) 100%),
    #312e2b;
  border-radius: 40px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4);
  display: flex;
  flex-direction: column;
  border: 1px solid rgba(255, 255, 255, 0.06);
  color: #ffffff;
}

.nav-bar {
  height: 87px;
  display: flex;
  flex-direction: column;
  font-weight: 700;
}

.status-bar {
  height: 47px;
  position: relative;
}

.status-right {
  position: absolute;
  right: 14.67px;
  top: 17.33px;
  width: 66.661px;
  height: 11.336px;
}

.status-right img {
  width: 100%;
  height: 100%;
  display: block;
}

.nav-header {
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 4px;
  backdrop-filter: blur(12px);
  position: relative;
}

.nav-title {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 215px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  font-size: 22px;
  font-weight: 700;
  line-height: 28px;
  color: #fff;
  pointer-events: none;
}

.nav-icon-button {
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
}

.nav-actions {
  display: flex;
}

.nav-icon-button img {
  width: 20px;
  height: 20px;
  display: block;
  opacity: 0.5;
  filter: drop-shadow(0 1px 0 rgba(0, 0, 0, 0.2));
}

.eval-section {
  /* Component handles its own padding */
}

.coach-area {
  padding: 12px;
  display: flex;
  align-items: flex-end;
}

.coach-bubble {
  flex: 1;
  background: #f7f7ff;
  border-radius: 12px;
  padding: 10px 12px;
  font-size: 12px;
  color: #333333;
}

.board-area {
  /* Hugs the board */
}

.content-area {
  flex: 1;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
  justify-content: center;
  align-items: center;
}

.marker-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 12px;
  color: rgba(255, 255, 255, 0.7);
  padding: 0 12px;
}

.classification-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 12px;
  color: rgba(255, 255, 255, 0.7);
  padding: 0 12px;
}

.bottom-region {
  flex: 1;
  padding: 12px;
  font-size: 12px;
  color: rgba(255, 255, 255, 0.7);
  display: grid;
  gap: 12px;
}

.skill-earned {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  padding: 10px 12px;
  display: grid;
  gap: 8px;
  color: #ffffff;
}

.skill-earned-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-weight: 600;
  color: #ffffff;
}

.skill-counter {
  color: #b2e068;
}

.skill-notes {
  font-size: 11px;
  color: rgba(255, 255, 255, 0.6);
}

.move-list-transition {
  display: grid;
  gap: 8px;
}

.bottom-sheet-overlay {
  position: relative;
}

.sheet-content {
  display: grid;
  gap: 12px;
}

.sheet-header {
  display: flex;
  align-items: center;
  gap: 12px;
}

.sheet-icon {
  width: 36px;
  height: 36px;
  border-radius: 12px;
  background: #eef2ff;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
}

.sheet-title {
  font-weight: 600;
  color: #2f2f2f;
}

.sheet-description {
  font-size: 12px;
  color: #6a6a6a;
}

.fade-in {
  opacity: 0;
  animation: fadeIn 100ms cubic-bezier(0, 0, 0.4, 1) forwards;
}

.fade-out {
  opacity: 1;
  animation: fadeOut 100ms cubic-bezier(0, 0, 0.4, 1) forwards;
}

.slide-in {
  animation: slideIn 100ms cubic-bezier(0, 0, 0.4, 1);
  transform: translateY(24px);
  animation-fill-mode: forwards;
}

.slide-out {
  animation: slideOut 100ms cubic-bezier(0, 0, 0.4, 1);
  animation-fill-mode: forwards;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(24px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideOut {
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0;
    transform: translateY(-24px);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}

.tab-bar {
  display: flex;
  flex-direction: column;
  padding-bottom: 0;
}

.tabs-container {
  display: flex;
  align-items: flex-end;
  gap: 8px;
  padding: 0 12px;
}

.tab-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  min-width: 64px;
  height: 48px;
}

.tab-icon {
  width: 24px;
  height: 24px;
  filter: drop-shadow(0 1px 0 rgba(0, 0, 0, 0.14));
}

.tab-icon img {
  width: 100%;
  height: 100%;
}

.tab-label {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  font-size: 12px;
  font-weight: 600;
  line-height: 16px;
  color: rgba(255, 255, 255, 0.72);
  text-shadow: 0 1px 0 rgba(0, 0, 0, 0.14);
  text-align: center;
  white-space: nowrap;
}

.tab-cta {
  flex: 1;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 12px 16px;
  border-radius: 10px;
  border: none;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  background: url('/icons/btn-next-bg.png') center/cover;
  box-shadow: 
    0 1px 2px rgba(0, 0, 0, 0.14),
    0 2px 4px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(178, 224, 104, 0.4),
    inset 0 -1px 0 #45753c,
    inset 0 2px 4px rgba(178, 224, 104, 0.5),
    inset 0 -2px 4px rgba(69, 117, 60, 0.5);
}

.tab-cta span {
  font-family: 'Chess Sans', sans-serif;
  font-weight: 800;
  font-size: 22px;
  line-height: 28px;
  color: #fff;
  text-shadow: 0 1px 0 rgba(0, 0, 0, 0.2);
}

.home-indicator {
  height: 24px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  padding-bottom: 8px;
}

.home-indicator::after {
  content: '';
  width: 134px;
  height: 5px;
  background: rgba(255, 255, 255, 0.25);
  border-radius: 100px;
}

.components-showcase {
  width: 420px;
  display: grid;
  gap: 16px;
}

.components-showcase h2 {
  margin: 0;
  font-size: 20px;
}

.component-card {
  background: #ffffff;
  border-radius: 16px;
  padding: 16px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
  display: grid;
  gap: 10px;
}

.component-card h3 {
  margin: 0;
  font-size: 14px;
}
</style>
