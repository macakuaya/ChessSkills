<script setup>
import { computed } from 'vue'

const props = defineProps({
  size: { type: Number, default: 375 },
  pieces: { type: Array, default: () => [] },
  highlights: { type: Array, default: () => [] },
  lastMove: { type: Object, default: null }, // { from: 'e2', to: 'e4' }
  skillHighlight: { type: String, default: null }, // Square to highlight with skill animation (e.g., 'd6')
  skillHighlightLabel: { type: String, default: null }, // Label text for skill highlight (e.g., 'Skewer')
})

const files = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
const ranks = [8, 7, 6, 5, 4, 3, 2, 1]
const squareSize = computed(() => props.size / 8)

const squares = computed(() => {
  const result = []
  for (let rank of ranks) {
    for (let file of files) {
      result.push(`${file}${rank}`)
    }
  }
  return result
})

const isLightSquare = (square) => {
  const fileIndex = files.indexOf(square[0])
  const rankIndex = parseInt(square[1], 10) - 1
  return (fileIndex + rankIndex) % 2 === 1
}

const isHighlighted = (square) => {
  return props.highlights.includes(square)
}

const isLastMoveSquare = (square) => {
  if (!props.lastMove) return false
  return props.lastMove.from === square || props.lastMove.to === square
}

const hasSkillHighlight = (square) => {
  return props.skillHighlight === square
}

const getPieceOnSquare = (square) => {
  return props.pieces.find(p => p.square === square)
}

// Piece image paths (PNG from Figma)
// Supports both 'wK' and 'wk' formats
const getPieceImage = (piece) => {
  const type = piece.type.toLowerCase()
  const base = import.meta.env.BASE_URL
  return `${base}pieces/${type}.png`
}
</script>

<template>
  <div class="board-wrapper">
    <div class="board" :style="{ width: `${size}px`, height: `${size}px` }">
      <div 
        v-for="square in squares" 
        :key="square"
        class="square"
        :class="{
          light: isLightSquare(square),
          dark: !isLightSquare(square),
          highlighted: isHighlighted(square),
          'last-move': isLastMoveSquare(square),
        }"
        :data-square="square"
      >
        <!-- Skill Highlight Overlay -->
        <div 
          v-if="hasSkillHighlight(square)" 
          class="skill-highlight-overlay"
        ></div>
        
        <!-- Skill Star Icon (separate from overlay to avoid opacity inheritance) -->
        <svg 
          v-if="hasSkillHighlight(square)" 
          class="skill-star-icon" 
          viewBox="0 0 20 20" 
          fill="none"
        >
          <path d="M6.56624 15.6425C6.14456 15.9236 5.64255 15.5622 5.78311 15.0803L6.82729 11.1244L3.6546 8.55415C3.23291 8.21278 3.57427 7.65053 3.9558 7.61037L8.05219 7.38949L9.51805 3.57423C9.59837 3.37342 9.79917 3.23286 10.0201 3.23286C10.2209 3.23286 10.4217 3.35334 10.502 3.57423L11.9678 7.38949L16.0442 7.61037C16.5662 7.63045 16.6867 8.27302 16.3454 8.53407L13.1727 11.1244L14.2168 15.0803C14.3574 15.5823 13.7751 15.8634 13.4337 15.6425L9.99998 13.4337L6.56624 15.6425Z" fill="white"/>
        </svg>
        
        <!-- Skill Label Bubble -->
        <div 
          v-if="hasSkillHighlight(square) && skillHighlightLabel" 
          class="skill-label-bubble"
        >
          {{ skillHighlightLabel }}
        </div>

        <!-- Piece -->
        <img 
          v-if="getPieceOnSquare(square)" 
          class="piece"
          :src="getPieceImage(getPieceOnSquare(square))"
          :alt="getPieceOnSquare(square).type"
          draggable="false"
        />

        <!-- File label (bottom row) -->
        <span v-if="square[1] === '1'" class="coord file-coord">{{ square[0] }}</span>
        
        <!-- Rank label (left column) -->
        <span v-if="square[0] === 'a'" class="coord rank-coord">{{ square[1] }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.board-wrapper {
  display: inline-block;
}

.board {
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  grid-template-rows: repeat(8, 1fr);
}

.square {
  position: relative;
  aspect-ratio: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.square.dark {
  background: #779556;
}

.square.light {
  background: #EBECD0;
}

.square.highlighted {
  background: #EDDA93;
}

.square.last-move.dark {
  background: #BBCB44;
}

.square.last-move.light {
  background: #F6F669;
}

.piece {
  width: 100%;
  height: 100%;
  object-fit: contain;
  z-index: 1;
  cursor: pointer;
  user-select: none;
  pointer-events: none;
}

.coord {
  position: absolute;
  font-family: 'SF Pro Text', -apple-system, system-ui, sans-serif;
  font-size: 10px;
  font-weight: 600;
  pointer-events: none;
}

.file-coord {
  bottom: 2px;
  right: 3px;
}

.rank-coord {
  top: 2px;
  left: 3px;
}

.square.dark .coord {
  color: #EBECD0;
}

.square.light .coord {
  color: #779556;
}

/* Skill Highlight Overlay */
.skill-highlight-overlay {
  position: absolute;
  inset: 0;
  background: #E3AA24; /* color-gold-300 */
  opacity: 0;
  z-index: 2;
  pointer-events: none;
  animation: skill-highlight-fade-in 300ms cubic-bezier(0, 0, 0.4, 1) forwards;
}

@keyframes skill-highlight-fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 0.8;
  }
}

/* Skill Star Icon */
.skill-star-icon {
  position: absolute;
  left: 50%;
  z-index: 3;
  pointer-events: none;
  filter: drop-shadow(0px 1.2px 0px rgba(0, 0, 0, 0.25));
  /* Animation: scale 20px → 24px, move from center to slightly below center, fade in */
  animation: skill-star-animate 300ms cubic-bezier(0, 0, 0.4, 1) forwards;
}

@keyframes skill-star-animate {
  from {
    opacity: 0.1;
    width: 20px;
    height: 20px;
    top: 50%;
    transform: translate(-50%, -50%);
  }
  to {
    opacity: 1;
    width: 24px;
    height: 24px;
    top: 55%;
    transform: translate(-50%, -50%);
  }
}

/* Skill Label Bubble */
.skill-label-bubble {
  position: absolute;
  left: 90%;
  z-index: 4;
  pointer-events: none;
  background: white;
  padding: 0 6px;
  border-radius: 10px;
  box-shadow: 0px 1.1px 0px rgba(0, 0, 0, 0.15);
  font-family: 'Chess Sans', system-ui, sans-serif;
  font-size: 11px;
  font-weight: 800;
  line-height: 20px;
  color: #cf8d1b; /* color-gold-400 */
  white-space: nowrap;
  /* Center of pill anchored at 90%, grows equally left and right */
  /* Animation: move from center to top, fade in */
  animation: skill-label-animate 300ms cubic-bezier(0, 0, 0.4, 1) forwards;
}

@keyframes skill-label-animate {
  from {
    opacity: 0.1;
    top: 50%;
    transform: translate(-50%, -50%);
  }
  to {
    opacity: 1;
    top: -6px;
    transform: translate(-50%, 0);
  }
}
</style>
