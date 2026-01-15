<script setup>
const props = defineProps({
  evalText: { type: String, default: '' },
  blackWidth: { type: Number, default: 50 },
  whiteWidth: { type: Number, default: 50 },
})
</script>

<template>
  <div class="eval-bar-wrapper">
    <div class="eval-track">
      <div class="eval-segment black" :style="{ flexGrow: props.blackWidth }">
        <span class="eval-text">{{ evalText || '—' }}</span>
      </div>
      <div class="eval-segment white" :style="{ width: `${props.whiteWidth}px` }" />
    </div>
  </div>
</template>

<style scoped>
.eval-bar-wrapper {
  width: 100%;
  padding: 12px 12px 0 12px;
}

.eval-track {
  width: 100%;
  height: 20px;
  display: flex;
  border-radius: 3px;
  overflow: hidden;
}

.eval-segment.black {
  background: #403d39;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 0 4px;
  min-width: 0;
  flex-grow: 1;
}

.eval-segment.white {
  background: #ffffff;
  flex-shrink: 0;
  transition: width 0.3s ease-out;
}

.eval-text {
  color: #ffffff;
  font-family: 'SF Pro Text', -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
  font-weight: 600;
  font-size: 11px;
  line-height: 13px;
  white-space: nowrap;
}
</style>
