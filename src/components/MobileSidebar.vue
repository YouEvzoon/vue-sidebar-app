<script setup>
import { markRaw } from 'vue'
import InicioView from './InicioView.vue'
import AnaliticasView from './AnaliticasView.vue'
import ProyectosView from './ProyectosView.vue'
import AjustesView from './AjustesView.vue'

const props = defineProps({
  activeItem: {
    type: String,
    default: 'Inicio',
  },
})

const emit = defineEmits(['select-component'])

const menuItems = [
  { name: 'Inicio', iconName: 'home', component: markRaw(InicioView) },
  { name: 'Analíticas', iconName: 'bar_chart', component: markRaw(AnaliticasView) },
  { name: 'Proyectos', iconName: 'folder', component: markRaw(ProyectosView) },
  { name: 'Ajustes', iconName: 'settings', component: markRaw(AjustesView) },
]

const handleSelect = (item) => {
  emit('select-component', item.component, item.name)
}
</script>

<template>
  <nav class="mobile-navbar" aria-label="Navegación móvil">
    <button
      v-for="item in menuItems"
      :key="item.name"
      type="button"
      class="nav-item"
      :class="{ active: props.activeItem === item.name }"
      @click="handleSelect(item)"
    >
      <span class="material-symbols-rounded icon">{{ item.iconName }}</span>
      <span class="label">{{ item.name }}</span>
    </button>
  </nav>
</template>

<style scoped>
.mobile-navbar {
  position: fixed;
  left: 50%;
  bottom: 1rem;
  transform: translateX(-50%);
  width: min(90vw, 420px);
  display: none;
  align-items: center;
  justify-content: space-between;
  gap: 0.4rem;
  padding: 0.7rem 0.7rem 0.8rem;
  border-radius: 28px;
  background: var(--bg-sidebar);
  border: 1px solid rgba(255, 255, 255, 0.06);
  box-shadow: 0 18px 35px rgba(0, 0, 0, 0.18);
  z-index: 30;
}

@media (max-width: 767px) {
  .mobile-navbar {
    display: flex;
  }
}

.nav-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.35rem;
  min-height: 62px;
  border: none;
  background: transparent;
  color: var(--text-muted);
  cursor: pointer;
  border-radius: 18px;
  transition: background-color 0.2s ease, transform 0.2s ease, color 0.2s ease;
}

.nav-item:hover {
  background: var(--hover-bg);
}

.nav-item.active {
  background: var(--primary-accent-rgb, rgba(137, 180, 250, 0.18));
  color: var(--text-main);
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.04);
}

.icon {
  font-size: 1.45rem;
  line-height: 1;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: currentColor;
}

.label {
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.01em;
  line-height: 1;
  color: currentColor;
}
</style>
