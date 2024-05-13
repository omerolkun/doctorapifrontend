<script setup>
import { ref, computed } from 'vue'
import HelloWorld from './components/HelloWorld.vue';
import TheWelcome from './components/TheWelcome.vue';
import WelcomeItem from './components/WelcomeItem.vue'
import Attach from './components/Attach.vue';


const routes = {
  '/hospital': HelloWorld,
  '/doctor': TheWelcome,
  '/' : WelcomeItem,
  '/junction': Attach

}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})
</script>

<template>
  <a href="#/hospital">Hastane</a> |
  <a href="#/doctor">Doktor</a> |
  <a href="#/">Hello page</a> | 
  <a href="#/junction">final</a>

  <component :is="currentView" />
</template>