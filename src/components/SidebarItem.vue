<script setup>
const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
  isCollapsed: {
    type: Boolean,
    default: false,
  },
  isActive: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['select'])

const handleSelect = () => {
  emit('select', props.item)
}
</script>

<template>
  <button
    type="button"
    class="nav-item"
    :class="{ active: isActive, collapsed: isCollapsed }"
    @click="handleSelect"
  >
    <span class="material-symbols-rounded icon">{{ item.iconName }}</span>
    <span v-if="!isCollapsed" class="label">{{ item.name }}</span>
  </button>
</template>

<style scoped>
.nav-item {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.8rem 0.9rem;
  border: none;
  border-radius: 10px;
  background: transparent;
  color: var(--text-main);
  cursor: pointer;
  transition: background-color 0.2s ease, transform 0.2s ease;
  text-align: left;
}

.nav-item:hover {
  background-color: var(--hover-bg);
}

.nav-item.active {
  background-color: var(--primary-accent-rgb, rgba(110, 118, 255, 0.18));
  color: var(--text-main);
}

.nav-item.collapsed {
  justify-content: center;
}

.icon {
  font-size: 22px;
  line-height: 1;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: currentColor;
  user-select: none;
}

.label {
  font-size: 0.95rem;
  white-space: nowrap;
}
</style>