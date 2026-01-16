<script setup>
const props = defineProps({
  skillName: { type: String, default: 'Skewers' },
  current: { type: Number, default: 1 },
  max: { type: Number, default: 10 },
  icon: { type: String, default: null },
  visible: { type: Boolean, default: false },
})

const baseUrl = import.meta.env.BASE_URL

function getProgressPercent() {
  return Math.round((props.current / props.max) * 100)
}
</script>

<template>
  <div class="skill-earned" :class="{ visible }">
    <!-- Skill Icon -->
    <div class="skill-icon">
      <img v-if="icon" :src="`${baseUrl}icons/${icon}.svg`" :alt="skillName" />
      <div v-else class="skill-icon-placeholder">
        <svg viewBox="0 0 40 40" fill="none">
          <rect x="2" y="2" width="36" height="36" rx="2" stroke="rgba(255,255,255,0.3)" stroke-width="2" stroke-dasharray="4 4" fill="none"/>
        </svg>
      </div>
    </div>
    
    <!-- Skill Info -->
    <div class="skill-info">
      <!-- Label + Counter -->
      <div class="skill-header">
        <span class="skill-name">{{ skillName }}</span>
        <span class="skill-counter">
          <span class="current">{{ current }}</span>
          <span class="separator">/</span>
          <span class="max">{{ max }}</span>
        </span>
      </div>
      
      <!-- Progress Bar -->
      <div class="progress-bar">
        <div class="progress-bg"></div>
        <div 
          class="progress-fill" 
          :style="{ width: getProgressPercent() + '%' }"
        ></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.skill-earned {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 12px;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  opacity: 0;
  pointer-events: none;
  transition: opacity 150ms cubic-bezier(0, 0, 0.2, 1);
}

.skill-earned.visible {
  opacity: 1;
  pointer-events: auto;
}

.skill-icon {
  width: 40px;
  height: 40px;
  flex-shrink: 0;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.skill-icon img {
  width: 40px;
  height: 40px;
  object-fit: contain;
}

.skill-icon-placeholder {
  width: 40px;
  height: 40px;
}

.skill-icon-placeholder svg {
  width: 100%;
  height: 100%;
}

.skill-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8px;
  min-width: 0;
}

.skill-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  line-height: 20px;
}

.skill-name {
  font-family: 'Chess Sans', system-ui, sans-serif;
  font-size: 17px;
  font-weight: 700;
  line-height: 20px;
  color: white;
  white-space: nowrap;
}

.skill-counter {
  display: flex;
  align-items: center;
  font-family: 'Chess Sans', system-ui, sans-serif;
  font-size: 17px;
  line-height: 20px;
  color: white;
  text-align: right;
}

.skill-counter .current {
  font-weight: 700;
}

.skill-counter .separator {
  font-weight: 800;
}

.skill-counter .max {
  font-weight: 700;
}

/* Progress Bar */
.progress-bar {
  position: relative;
  height: 16px;
  width: 100%;
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
</style>
