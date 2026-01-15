<script setup>
const props = defineProps({
  skillName: { type: String, default: '' },
  count: { type: Number, default: 0 },
  variant: { type: String, default: 'default' },
})
</script>

<template>
  <div class="skill-leave" :data-variant="variant">
    <span class="label">{{ skillName || 'Skill' }}</span>
    <span class="count">+{{ count }}</span>
  </div>
</template>

<style scoped>
.skill-leave {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 12px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.12);
  font-size: 12px;
  font-weight: 600;
}

.skill-leave[data-variant='warning'] {
  background: #fff7e6;
  border-color: #ffe0a3;
}

.label {
  color: rgba(255, 255, 255, 0.9);
}

.count {
  color: #b2e068;
}
</style>
