<script setup>
import { ref, markRaw, watch, onMounted } from 'vue'
import Sidebar from './components/Sidebar.vue'
import MobileSidebar from './components/MobileSidebar.vue'
import InicioView from './components/InicioView.vue'
import AnaliticasView from './components/AnaliticasView.vue'
import ProyectosView from './components/ProyectosView.vue'
import AjustesView from './components/AjustesView.vue'

const currentComponent = ref(markRaw(InicioView))
const activeItem = ref('Inicio')
const isDarkMode = ref(true)
const mobileMenuOpen = ref(false)

const applyTheme = () => {
  document.body.classList.toggle('light-theme', !isDarkMode.value)
}

const selectComponent = (component, itemName) => {
  currentComponent.value = component
  activeItem.value = itemName || 'Inicio'
  mobileMenuOpen.value = false
}

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value
}

watch(isDarkMode, (value) => {
  if (typeof window !== 'undefined') {
    localStorage.setItem('theme-dark', JSON.stringify(value))
  }

  applyTheme()
}, { immediate: true })

onMounted(() => {
  const savedTheme = typeof window !== 'undefined'
    ? localStorage.getItem('theme-dark')
    : null

  if (savedTheme !== null) {
    isDarkMode.value = JSON.parse(savedTheme)
  }

  applyTheme()
})
</script>

<template>
  <div class="layout">
    <button class="mobile-menu-toggle" type="button" aria-label="Abrir menú" @click="mobileMenuOpen = true">
      <span class="material-symbols-rounded">menu</span>
    </button>

    <Sidebar
      class="desktop-sidebar"
      :active-item="activeItem"
      :is-dark-mode="isDarkMode"
      @select-component="selectComponent"
      @toggle-theme="toggleTheme"
    />

    <MobileSidebar
      v-if="mobileMenuOpen"
      :active-item="activeItem"
      :is-dark-mode="isDarkMode"
      @select-component="selectComponent"
      @toggle-theme="toggleTheme"
      @close="mobileMenuOpen = false"
    />

    <main class="content">
      <component :is="currentComponent" />
    </main>
  </div>
</template>

<style scoped>
.layout {
  display: flex;
  min-height: 100vh;
}

.content {
  flex: 1;
  padding: 2rem;
}

.mobile-menu-toggle {
  display: none;
  position: fixed;
  top: 1rem;
  left: 1rem;
  z-index: 50;
  width: 46px;
  height: 46px;
  border: none;
  border-radius: 12px;
  background-color: var(--bg-sidebar);
  color: var(--text-main);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.18);
  cursor: pointer;
}

@media (max-width: 767px) {
  .layout {
    display: block;
  }

  .desktop-sidebar {
    display: none;
  }

  .mobile-menu-toggle {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .content {
    padding: 5rem 1rem 1.5rem;
  }
}
</style>