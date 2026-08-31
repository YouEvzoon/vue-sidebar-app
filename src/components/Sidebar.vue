<script setup>
import { ref, watch, markRaw } from 'vue'
import SidebarItem from './SidebarItem.vue'
import InicioView from './InicioView.vue'
import AnaliticasView from './AnaliticasView.vue'
import ProyectosView from './ProyectosView.vue'
import AjustesView from './AjustesView.vue'

const isCollapsed = ref(false)
const isDarkMode = ref(true)
const activeItem = ref('Inicio')

const savedCollapsed = typeof window !== 'undefined'
  ? localStorage.getItem('sidebar-collapsed')
  : null

const savedTheme = typeof window !== 'undefined'
  ? localStorage.getItem('theme-dark')
  : null

if (savedCollapsed !== null) {
  isCollapsed.value = JSON.parse(savedCollapsed)
}

if (savedTheme !== null) {
  isDarkMode.value = JSON.parse(savedTheme)
}

const emit = defineEmits(['select-component'])

const toggleSidebar = () => {
  isCollapsed.value = !isCollapsed.value
}

const handleSelect = (item) => {
  activeItem.value = item.name
  emit('select-component', item.component)
}

watch(isCollapsed, (newValue) => {
  if (typeof window !== 'undefined') {
    localStorage.setItem('sidebar-collapsed', JSON.stringify(newValue))
  }
}, { immediate: true })

watch(isDarkMode, (newValue) => {
  if (typeof window !== 'undefined') {
    localStorage.setItem('theme-dark', JSON.stringify(newValue))
  }

  if (newValue) {
    document.body.classList.remove('light-theme')
  } else {
    document.body.classList.add('light-theme')
  }
}, { immediate: true })

const menuItems = [
  { name: 'Inicio', iconName: 'home', component: markRaw(InicioView) },
  { name: 'Analíticas', iconName: 'bar_chart', component: markRaw(AnaliticasView) },
  { name: 'Proyectos', iconName: 'folder', component: markRaw(ProyectosView) },
  { name: 'Ajustes', iconName: 'settings', component: markRaw(AjustesView) },
]
</script>

<template>
  <aside :class="['sidebar', { collapsed: isCollapsed }]">
    <button class="toggle-btn" @click="toggleSidebar" aria-label="Toggle Sidebar">
      <span class="icon">{{ isCollapsed ? '❯' : '❮' }}</span>
    </button>

    <div class="sidebar-header">
      <div class="logo-icon">
        <span class="material-symbols-rounded">bolt</span>
      </div>
      <span v-if="!isCollapsed" class="logo-text">CODIGO-PC</span>
    </div>

    <nav class="nav-menu">
      <SidebarItem
        v-for="item in menuItems"
        :key="item.name"
        :item="item"
        :is-collapsed="isCollapsed"
        :is-active="activeItem === item.name"
        @select="handleSelect"
      />
    </nav>

    <div class="sidebar-footer">
      <label class="theme-switch" :title="isCollapsed ? (isDarkMode ? 'Modo Oscuro' : 'Modo Claro') : ''">
        <input type="checkbox" v-model="isDarkMode" />
        <span class="slider">
          <span class="switch-icon material-symbols-rounded">{{ isDarkMode ? 'dark_mode' : 'light_mode' }}</span>
        </span>
      </label>
      <span v-if="!isCollapsed" class="theme-label">
        {{ isDarkMode ? 'Modo Oscuro' : 'Modo Claro' }}
      </span>
    </div>
  </aside>
</template>

<style scoped>
.sidebar {
  width: 240px;
  height: 100vh;
  background-color: var(--bg-sidebar);
  color: var(--text-main);
  display: flex;
  flex-direction: column;
  padding: 1rem 0.75rem;
  position: relative;
  transition: width 0.3s ease, background-color 0.3s ease;
  box-sizing: border-box;
}

.sidebar.collapsed {
  width: 70px;
}

.toggle-btn {
  position: absolute;
  top: 1.2rem;
  right: -12px;
  width: 26px;
  height: 26px;
  background-color: var(--primary-accent);
  color: var(--btn-text);
  border: none;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  z-index: 10;
}

.sidebar-header {
  display: flex;
  align-items: center;
  gap: 0.7rem;
  padding: 0.35rem 0.45rem 0.55rem;
  margin-bottom: 1.2rem;
  min-height: 2.5rem;
}

.logo-icon {
  width: auto;
  height: auto;
  display: flex;
  align-items: center;
  justify-content: center;
  background: transparent;
  box-shadow: none;
}

.logo-icon .material-symbols-rounded {
  font-size: 1rem;
  color: #22c55e;
}

.logo-text {
  color: var(--text-main);
  letter-spacing: 0.02em;
  font-size: 0.95rem;
  font-weight: 700;
  line-height: 1;
  transform: translateY(0.5px);
  white-space: nowrap;
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
  box-shadow: inset 0 2px 6px rgba(0,0,0,0.25);
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
  box-shadow: 0 3px 8px rgba(0,0,0,0.25);
}

input:checked + .slider {
  background: linear-gradient(135deg, #22c55e, #16a34a);
}

input:checked + .slider .switch-icon {
  transform: translateX(18px);
  color: #ffffff;
}
</style>