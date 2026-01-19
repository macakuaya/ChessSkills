<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  skillName: { type: String, default: 'Skewers' },
  current: { type: Number, default: 1 },
  max: { type: Number, default: 10 },
  icon: { type: String, default: null },
  visible: { type: Boolean, default: false },
})

const baseUrl = import.meta.env.BASE_URL

// Animated progress starts at current value, then grows +1
const animatedProgress = ref(0)

// Animated counter for slot machine effect
const animatedCounter = ref(0)
const isCounterAnimating = ref(false)

function getCurrentPercent() {
  return Math.round((props.current / props.max) * 100)
}

function getNextPercent() {
  return Math.round(((props.current + 1) / props.max) * 100)
}

// Watch visibility to trigger progress animation
watch(() => props.visible, (isVisible) => {
  if (isVisible) {
    // Start at current progress (1/10 = 10%)
    animatedProgress.value = getCurrentPercent()
    animatedCounter.value = props.current
    isCounterAnimating.value = false
    
    // After explosion completes (~1550ms from visibility), grow +1 point
    // Explosion starts at 1350ms from trigger, lasts 500ms
    // SkillEarned becomes visible at ~300ms from trigger
    // So: 1850ms - 300ms = 1550ms from visibility
    setTimeout(() => {
      animatedProgress.value = getNextPercent()
      isCounterAnimating.value = true
      animatedCounter.value = props.current + 1
    }, 1550)
  } else {
    // Reset when hidden
    animatedProgress.value = 0
    animatedCounter.value = 0
    isCounterAnimating.value = false
  }
})
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
          <span class="counter-slot" :class="{ animating: isCounterAnimating }">
            <span class="counter-value previous">{{ current }}</span>
            <span class="counter-value next">{{ current + 1 }}</span>
          </span>
          <span class="separator">/</span>
          <span class="max">{{ max }}</span>
        </span>
      </div>
      
      <!-- Progress Bar -->
      <div class="progress-bar">
        <div class="progress-bg"></div>
        <div 
          class="progress-fill" 
          :style="{ width: animatedProgress + '%' }"
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
  transform: translateY(24px);
  pointer-events: none;
  transition: opacity 100ms cubic-bezier(0, 0, 0.4, 1),
              transform 100ms cubic-bezier(0, 0, 0.4, 1);
}

.skill-earned.visible {
  opacity: 1;
  transform: translateY(0);
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

/* Slot machine counter */
.counter-slot {
  position: relative;
  display: inline-flex;
  flex-direction: column;
  height: 20px;
  overflow: hidden;
}

.counter-value {
  font-weight: 700;
  height: 20px;
  display: flex;
  align-items: center;
  transition: transform 500ms cubic-bezier(0, 0, 0.2, 1),
              opacity 500ms cubic-bezier(0, 0, 0.2, 1);
}

.counter-value.previous {
  opacity: 1;
  transform: translateY(0);
}

.counter-value.next {
  opacity: 0;
  transform: translateY(0);
}

/* When animating, slide up */
.counter-slot.animating .counter-value.previous {
  opacity: 0;
  transform: translateY(-20px);
}

.counter-slot.animating .counter-value.next {
  opacity: 1;
  transform: translateY(-20px);
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
  transition: width 500ms cubic-bezier(0, 0, 0.2, 1);
}
</style>
