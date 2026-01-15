<script setup>
import { computed } from 'vue'

const props = defineProps({
  size: { type: Number, default: 375 },
  pieces: { type: Array, default: () => [] },
  highlights: { type: Array, default: () => [] },
  lastMove: { type: Object, default: null }, // { from: 'e2', to: 'e4' }
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

const getPieceOnSquare = (square) => {
  return props.pieces.find(p => p.square === square)
}

// Piece image paths (PNG from Figma)
// Supports both 'wK' and 'wk' formats
const getPieceImage = (piece) => {
  const type = piece.type.toLowerCase()
  return `/pieces/${type}.png`
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
</style>
