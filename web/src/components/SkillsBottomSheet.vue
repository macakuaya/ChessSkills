<script setup>
import { ref } from 'vue'

const props = defineProps({
  open: { type: Boolean, default: true },
  skills: { 
    type: Array, 
    default: () => [
      { name: 'Royal Fork', current: 9, max: 10, icon: 'royal-fork' },
      { name: 'Absolute Pin', current: 5, max: 10, icon: 'absolute-pin' },
      { name: 'Trapped Piece', current: 1, max: 10, icon: 'trapped-piece' },
    ]
  }
})

const emit = defineEmits(['close'])

const activeTab = ref('tactics')

const tabs = [
  { id: 'openings', label: 'Openings' },
  { id: 'tactics', label: 'Tactics' },
  { id: 'strategy', label: 'Strategy' },
  { id: 'endgames', label: 'Endgames' },
]

// Skill icons - using placeholder SVGs for now
const skillIcons = {
  'royal-fork': `<svg viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M20 8L24 16H16L20 8Z" fill="white"/>
    <path d="M12 20L20 16L28 20L24 32H16L12 20Z" fill="white"/>
    <circle cx="20" cy="12" r="2" fill="white"/>
  </svg>`,
  'absolute-pin': `<svg viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M20 6L26 14H14L20 6Z" fill="white"/>
    <rect x="16" y="14" width="8" height="20" rx="1" fill="white"/>
    <circle cx="20" cy="10" r="3" fill="white"/>
  </svg>`,
  'trapped-piece': `<svg viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
    <rect x="8" y="28" width="24" height="4" rx="1" fill="white"/>
    <path d="M12 28V20L16 16L20 20L24 16L28 20V28H12Z" fill="white"/>
    <circle cx="14" cy="12" r="2" fill="white"/>
    <circle cx="26" cy="12" r="2" fill="white"/>
  </svg>`,
}

function getProgressPercent(current, max) {
  return Math.round((current / max) * 100)
}
</script>

<template>
  <div class="skills-bottom-sheet" :class="{ open }">
    <!-- Top Container: Handle + Title + Tabs -->
    <div class="sheet-top">
      <!-- Drag Handle -->
      <div class="drag-container">
        <div class="drag-handle"></div>
      </div>
      
      <!-- Title -->
      <div class="title-container">
        <h2 class="title">Skills</h2>
      </div>
      
      <!-- Tabs -->
      <div class="tabs-container">
        <button
          v-for="tab in tabs"
          :key="tab.id"
          class="tab"
          :class="{ active: activeTab === tab.id }"
          @click="activeTab = tab.id"
        >
          {{ tab.label }}
        </button>
      </div>
    </div>
    
    <!-- Skills List -->
    <div class="skills-container">
      <div class="skills-list">
        <div 
          v-for="skill in skills" 
          :key="skill.name"
          class="skill-item"
        >
          <!-- Skill Icon -->
          <div class="skill-icon" v-html="skillIcons[skill.icon] || skillIcons['royal-fork']"></div>
          
          <!-- Skill Info -->
          <div class="skill-info">
            <!-- Label + Counter -->
            <div class="skill-header">
              <span class="skill-name">{{ skill.name }}</span>
              <span class="skill-counter">
                <span class="current">{{ skill.current }}</span>
                <span class="separator">/</span>
                <span class="max">{{ skill.max }}</span>
              </span>
            </div>
            
            <!-- Progress Bar -->
            <div class="progress-bar">
              <div class="progress-bg"></div>
              <div 
                class="progress-fill" 
                :style="{ width: getProgressPercent(skill.current, skill.max) + '%' }"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Home Indicator -->
    <div class="home-indicator-container">
      <div class="home-indicator"></div>
    </div>
  </div>
</template>

<style scoped>
.skills-bottom-sheet {
  display: flex;
  flex-direction: column;
  width: 100%;
  background: rgba(38, 36, 33, 0.75);
  backdrop-filter: blur(25px);
  -webkit-backdrop-filter: blur(25px);
  border-radius: 10px 10px 0 0;
  overflow: hidden;
  transform: translateY(100%);
  opacity: 0;
  transition: transform 300ms cubic-bezier(0.4, 0, 0.2, 1),
              opacity 200ms cubic-bezier(0.4, 0, 0.2, 1);
}

.skills-bottom-sheet.open {
  transform: translateY(0);
  opacity: 1;
}

/* Top Container */
.sheet-top {
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

/* Drag Handle */
.drag-container {
  display: flex;
  justify-content: center;
  padding-top: 12px;
}

.drag-handle {
  width: 60px;
  height: 5px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 100px;
}

/* Title */
.title-container {
  padding: 0 12px;
  height: 24px;
  display: flex;
  align-items: center;
}

.title {
  font-family: 'Chess Sans', system-ui, sans-serif;
  font-size: 22px;
  font-weight: 800;
  line-height: 28px;
  color: white;
  margin: 0;
}

/* Tabs */
.tabs-container {
  display: flex;
  height: 56px;
  padding: 0;
}

.tab {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 56px;
  padding: 0 12px;
  background: transparent;
  border: none;
  border-bottom: 3px solid transparent;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  font-size: 14px;
  font-weight: 600;
  line-height: 16px;
  color: rgba(255, 255, 255, 0.72);
  cursor: pointer;
  transition: color 150ms ease, border-color 150ms ease;
}

.tab:hover {
  color: rgba(255, 255, 255, 0.85);
}

.tab.active {
  color: rgba(255, 255, 255, 0.85);
  border-bottom-color: #81b64c;
}

/* Skills Container */
.skills-container {
  padding: 0 12px;
}

.skills-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
  padding: 16px 0;
  max-width: 500px;
}

/* Skill Item */
.skill-item {
  display: flex;
  align-items: center;
  gap: 8px;
}

.skill-icon {
  width: 44px;
  height: 44px;
  flex-shrink: 0;
  border-radius: 3px;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.skill-icon :deep(svg) {
  width: 40px;
  height: 40px;
}

.skill-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.skill-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.skill-name {
  font-family: 'Chess Sans', system-ui, sans-serif;
  font-size: 17px;
  font-weight: 700;
  line-height: 20px;
  color: white;
}

.skill-counter {
  display: flex;
  align-items: center;
  font-family: 'Chess Sans', system-ui, sans-serif;
  font-size: 17px;
  line-height: 20px;
  color: white;
}

.skill-counter .current {
  font-weight: 700;
}

.skill-counter .separator {
  font-weight: 800;
  margin: 0 1px;
}

.skill-counter .max {
  font-weight: 700;
}

/* Progress Bar */
.progress-bar {
  position: relative;
  height: 16px;
  border-radius: 8px;
  overflow: hidden;
}

.progress-bg {
  position: absolute;
  inset: 0;
  background: #4b4847;
  border-radius: 8px;
}

.progress-fill {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  background: #81b64c;
  border-radius: 10px;
  transition: width 300ms cubic-bezier(0.4, 0, 0.2, 1);
}

/* Home Indicator */
.home-indicator-container {
  height: 24px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  padding-bottom: 8px;
}

.home-indicator {
  width: 134px;
  height: 5px;
  background: rgba(255, 255, 255, 0.25);
  border-radius: 100px;
}
</style>
