<script setup>
const props = defineProps({
  type: { type: String, default: 'good' },
})
</script>

<template>
  <span class="badge" :data-type="type">{{ type }}</span>
</template>

<style scoped>
.badge {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 2px 8px;
  border-radius: 999px;
  font-size: 10px;
  font-weight: 600;
  background: #e8e6ff;
  color: #4a3fd6;
}

.badge[data-type='brilliant'] {
  background: #fff2cc;
  color: #8a5b00;
}

.badge[data-type='blunder'] {
  background: #ffe0e0;
  color: #b00020;
}
</style>
