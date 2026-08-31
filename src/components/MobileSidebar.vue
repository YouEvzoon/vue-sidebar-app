<script setup>
import { markRaw } from 'vue'
import SidebarItem from './SidebarItem.vue'
import InicioView from './InicioView.vue'
import AnaliticasView from './AnaliticasView.vue'
import ProyectosView from './ProyectosView.vue'
import AjustesView from './AjustesView.vue'

const props = defineProps({
  activeItem: {
    type: String,
    default: 'Inicio',
  },
  isDarkMode: {
    type: Boolean,
    default: true,
  },
})

const emit = defineEmits(['select-component', 'toggle-theme', 'close'])

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
  <div class="mobile-sidebar-wrapper">
    <div class="mobile-backdrop" @click="emit('close')" />

    <aside class="mobile-sidebar">
      <div class="mobile-header">
        <div class="logo-icon">
          <span class="material-symbols-rounded">bolt</span>
        </div>
        <span class="logo-text">CODIGO-PC</span>
        <button class="close-btn" type="button" @click="emit('close')" aria-label="Cerrar menú">
          <span class="material-symbols-rounded">close</span>
        </button>
      </div>

      <nav class="nav-menu">
        <SidebarItem
          v-for="item in menuItems"
          :key="item.name"
          :item="item"
          :is-collapsed="false"
          :is-active="props.activeItem === item.name"
          @select="handleSelect"
        />
      </nav>

      <div class="sidebar-footer">
        <label class="theme-switch" :title="props.isDarkMode ? 'Modo Oscuro' : 'Modo Claro'">
          <input type="checkbox" :checked="props.isDarkMode" @change="emit('toggle-theme')" />
          <span class="slider">
            <span class="switch-icon material-symbols-rounded">{{ props.isDarkMode ? 'dark_mode' : 'light_mode' }}</span>
          </span>
        </label>
        <span class="theme-label">{{ props.isDarkMode ? 'Modo Oscuro' : 'Modo Claro' }}</span>
      </div>
    </aside>
  </div>
</template>

<style scoped>
.mobile-sidebar-wrapper {
  position: fixed;
  inset: 0;
  z-index: 40;
}

.mobile-backdrop {
  position: absolute;
  inset: 0;
  background: rgba(15, 23, 42, 0.55);
}

.mobile-sidebar {
  position: absolute;
  inset: 0 auto 0 0;
  width: min(82vw, 280px);
  height: 100vh;
  background-color: var(--bg-sidebar);
  color: var(--text-main);
  display: flex;
  flex-direction: column;
  padding: 1rem 0.85rem;
  box-sizing: border-box;
  box-shadow: 0 18px 50px rgba(0, 0, 0, 0.3);
}

.mobile-header {
  display: flex;
  align-items: center;
  gap: 0.65rem;
  padding: 0.25rem 0.35rem 1rem;
  min-height: 3rem;
}

.logo-icon {
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo-icon .material-symbols-rounded {
  font-size: 1rem;
  color: #22c55e;
}

.logo-text {
  flex: 1;
  color: var(--text-main);
  letter-spacing: 0.02em;
  font-size: 0.95rem;
  font-weight: 700;
  white-space: nowrap;
}

.close-btn {
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
  border-radius: 10px;
  background: transparent;
  color: var(--text-main);
  cursor: pointer;
}

.nav-menu {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex: 1;
}

.sidebar-footer {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 0.5rem;
  border-top: 1px solid var(--hover-bg);
}

.theme-label {
  font-size: 0.9rem;
  color: var(--text-muted);
  white-space: nowrap;
}

.theme-switch {
  position: relative;
  display: inline-block;
  width: 42px;
  height: 24px;
  flex-shrink: 0;
}

.theme-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, #3b4252, #4c566a);
  transition: 0.3s ease;
  border-radius: 999px;
  box-shadow: inset 0 2px 6px rgba(0, 0, 0, 0.25);
}

.switch-icon {
  position: absolute;
  height: 18px;
  width: 18px;
  left: 3px;
  bottom: 3px;
  background-color: var(--bg-card);
  color: var(--primary-accent);
  transition: 0.3s ease;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.25);
}

input:checked + .slider {
  background: linear-gradient(135deg, #22c55e, #16a34a);
}

input:checked + .slider .switch-icon {
  transform: translateX(18px);
  color: #ffffff;
}
</style>
