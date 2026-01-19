<script setup>
import { CcButton, CcIcon } from '@chesscom/design-system'

const props = defineProps({
  visible: {
    type: Boolean,
    default: false
  },
  skillName: {
    type: String,
    default: 'Trapped Piece'
  },
  skillDescription: {
    type: String,
    default: 'A piece that has no safe squares to move to and will be captured, usually because it\'s surrounded by enemy pawns or pieces blocking its escape routes.'
  },
  skillImage: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['close', 'continue'])

function handleClose() {
  emit('close')
}

function handleContinue() {
  emit('continue')
}
</script>

<template>
  <Transition name="modal-fade">
    <div v-if="visible" class="skill-unlocked-modal">
      <!-- Status Bar placeholder -->
      <div class="status-bar-space"></div>
      
      <!-- Close Button -->
      <button class="close-button" @click="handleClose">
        <CcIcon name="mark-cross" :size="16" />
      </button>
      
      <!-- Content -->
      <div class="modal-content">
        <div class="main-content">
          <!-- Skill Image -->
          <div class="skill-image-container">
            <img 
              v-if="skillImage" 
              :src="skillImage" 
              :alt="skillName"
              class="skill-image"
            />
            <div v-else class="skill-image-placeholder">
            </div>
          </div>
          
          <!-- Text Content -->
          <div class="text-content">
            <div class="headline-container">
              <h1 class="skill-title">{{ skillName }}</h1>
            </div>
            <p class="skill-description">{{ skillDescription }}</p>
          </div>
        </div>
        
        <!-- CTA Button -->
        <div class="cta-container">
          <CcButton variant="primary" size="x-large" class="continue-btn" @click="handleContinue">
            Continue
          </CcButton>
        </div>
      </div>
      
      <!-- Home Indicator -->
      <div class="home-indicator">
        <div class="indicator-bar"></div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.skill-unlocked-modal {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, #312e2b 0%, #262421 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 100;
  overflow: hidden;
}

.status-bar-space {
  height: 47px;
  width: 100%;
  flex-shrink: 0;
}

.close-button {
  position: absolute;
  top: 47px;
  right: 0;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: transparent;
  border: none;
  cursor: pointer;
  color: rgba(255, 255, 255, 1);
  padding: 12px;
}

.close-button:hover {
  opacity: 0.8;
}

.modal-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  padding: 0 12px;
  min-height: 0;
}

.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 24px;
  max-width: 460px;
  width: 100%;
  padding-bottom: 24px;
  min-height: 0;
}

.skill-image-container {
  width: 100%;
  aspect-ratio: 1 / 1;
  flex-shrink: 0;
  max-height: 366px;
}

.skill-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}

.skill-image-placeholder {
  width: 100%;
  height: 100%;
  background: #3d3a36;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.text-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
  max-width: 500px;
  width: 100%;
}

.headline-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  width: 100%;
}

.skill-title {
  font-family: 'Chess Sans', sans-serif;
  font-weight: 800;
  font-size: 28px;
  line-height: 32px;
  color: white;
  text-align: center;
  margin: 0;
}

.skill-description {
  font-family: 'Inter', sans-serif;
  font-weight: 600;
  font-size: 16px;
  line-height: 24px;
  color: rgba(255, 255, 255, 0.85);
  text-align: center;
  margin: 0;
}

.cta-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  max-width: 460px;
  width: 100%;
  padding-bottom: 12px;
}

.continue-btn {
  width: 100%;
}

.home-indicator {
  height: 24px;
  width: 100%;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  padding-bottom: 8px;
  flex-shrink: 0;
}

.indicator-bar {
  width: 134px;
  height: 5px;
  background: rgba(255, 255, 255, 0.25);
  border-radius: 100px;
}

/* Modal transition */
.modal-fade-enter-active {
  transition: opacity 300ms ease-out;
}

.modal-fade-leave-active {
  transition: opacity 200ms ease-in;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}
</style>
