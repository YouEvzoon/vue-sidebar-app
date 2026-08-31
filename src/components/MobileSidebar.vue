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
  { name: 'Buscar', iconName: 'search', component: markRaw(AnaliticasView) },
  { name: 'Analíticas', iconName: 'bar_chart', component: markRaw(AnaliticasView) },
  { name: 'Historial', iconName: 'history', component: markRaw(ProyectosView) },
  { name: 'Perfil', iconName: 'person', component: markRaw(AjustesView) },
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
      :class="{ active: props.activeItem === item.name || (item.name === 'Inicio' && props.activeItem === 'Home') }"
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
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.4rem;
  padding: 0.7rem 0.7rem 0.8rem;
  border-radius: 28px;
  background: rgba(17, 18, 23, 0.96);
  border: 1px solid rgba(255, 255, 255, 0.06);
  box-shadow: 0 18px 35px rgba(0, 0, 0, 0.28);
  z-index: 30;
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
  color: rgba(255, 255, 255, 0.68);
  cursor: pointer;
  border-radius: 18px;
  transition: all 0.2s ease;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.04);
}

.nav-item.active {
  background: linear-gradient(180deg, rgba(129, 105, 255, 0.9), rgba(117, 90, 242, 0.78));
  color: #fff;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.18);
}

.icon {
  font-size: 1.45rem;
  line-height: 1;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.label {
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.01em;
  line-height: 1;
}
</style>
