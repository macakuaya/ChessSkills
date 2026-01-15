<script setup>
const props = defineProps({
  title: { type: String, default: '' },
  content: { type: String, default: '' },
  overlayVisible: { type: Boolean, default: false },
  open: { type: Boolean, default: true },
})
</script>

<template>
  <div class="sheet-wrapper">
    <div v-if="overlayVisible" class="overlay" />
    <div class="sheet" :class="{ open }">
      <div class="grabber" />
      <h3>{{ title || 'Bottom Sheet' }}</h3>
      <slot>
        <p v-if="content" class="content">{{ content }}</p>
        <p v-else class="empty">No content</p>
      </slot>
    </div>
  </div>
</template>

<style scoped>
.sheet-wrapper {
  position: relative;
}

.overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.35);
  border-radius: 12px;
}

.sheet {
  position: relative;
  margin-top: 12px;
  background: #ffffff;
  border-radius: 16px;
  padding: 16px;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
  transform: translateY(24px);
  opacity: 0;
  transition: transform 160ms cubic-bezier(0, 0, 0.4, 1),
    opacity 160ms cubic-bezier(0, 0, 0.4, 1);
}

.sheet.open {
  transform: translateY(0);
  opacity: 1;
}

.grabber {
  width: 40px;
  height: 4px;
  background: #e0e0e0;
  border-radius: 999px;
  margin: 0 auto 12px;
}

.content {
  font-size: 12px;
  color: #4d4d4d;
}

.empty {
  font-size: 12px;
  color: #9a9a9a;
}
</style>
