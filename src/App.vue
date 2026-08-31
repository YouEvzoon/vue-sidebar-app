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

const applyTheme = () => {
  document.body.classList.toggle('light-theme', !isDarkMode.value)
}

const selectComponent = (component, itemName) => {
  currentComponent.value = component
  activeItem.value = itemName || 'Inicio'
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
    <Sidebar
      class="desktop-sidebar"
      :active-item="activeItem"
      :is-dark-mode="isDarkMode"
      @select-component="selectComponent"
      @toggle-theme="toggleTheme"
    />

    <MobileSidebar
      :active-item="activeItem"
      @select-component="selectComponent"
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

@media (max-width: 767px) {
  .layout {
    display: block;
  }

  .desktop-sidebar {
    display: none;
  }

  .content {
    padding: 5rem 1rem 5.5rem;
  }
}
</style>