<script setup>
import { ref, markRaw } from 'vue'
import Sidebar from './components/Sidebar.vue'
import InicioView from './components/InicioView.vue'
import AnaliticasView from './components/AnaliticasView.vue'
import ProyectosView from './components/ProyectosView.vue'
import AjustesView from './components/AjustesView.vue'

const currentComponent = ref(markRaw(InicioView))

const selectComponent = (component) => {
  currentComponent.value = component
}
</script>

<template>
  <div class="layout">
    <Sidebar @select-component="selectComponent" />

    <main class="content">
      <component :is="currentComponent" />
    </main>
  </div>
</template>

<style scoped>
.layout {
  display: flex;
}

.content {
  flex: 1;
  padding: 2rem;
}
</style>