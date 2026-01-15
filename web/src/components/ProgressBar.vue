<script setup>
const props = defineProps({
  value: { type: Number, default: 0 },
  max: { type: Number, default: 100 },
  label: { type: String, default: '' },
  showExplosion: { type: Boolean, default: false },
})

const percent = Math.min(100, Math.max(0, (props.value / props.max) * 100 || 0))
</script>

<template>
  <div class="progress">
    <div class="header">
      <span class="label">{{ label || 'Progress' }}</span>
      <span class="value">{{ value }}/{{ max }}</span>
    </div>
    <div class="track">
      <div class="fill" :style="{ width: `${percent}%` }" />
      <div v-if="showExplosion" class="explosion">✨</div>
    </div>
  </div>
</template>

<style scoped>
.progress {
  display: grid;
  gap: 6px;
  font-size: 12px;
  color: rgba(255, 255, 255, 0.85);
}

.header {
  display: flex;
  justify-content: space-between;
  color: inherit;
}

.label {
  font-weight: 600;
}

.track {
  position: relative;
  height: 12px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 999px;
  overflow: hidden;
}

.fill {
  height: 100%;
  background: #81b64c;
}

.explosion {
  position: absolute;
  right: 6px;
  top: -8px;
}
</style>
