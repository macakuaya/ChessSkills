<script setup>
const props = defineProps({
  type: { type: String, default: 'skewer' },
  label: { type: String, default: '' },
  position: { type: String, default: '' },
})
</script>

<template>
  <div class="marker" :data-type="type">
    <span class="label">{{ label || type }}</span>
    <span v-if="position" class="position">{{ position }}</span>
  </div>
</template>

<style scoped>
.marker {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 4px 8px;
  border-radius: 999px;
  font-size: 11px;
  font-weight: 600;
  background: #ececff;
  color: #3f36c5;
}

.marker[data-type='brilliant'] {
  background: #fff2cc;
  color: #8a5b00;
}

.position {
  color: #6b6b6b;
  font-weight: 500;
}
</style>
