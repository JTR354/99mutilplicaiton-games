<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import HelloWorld from './components/HelloWorld.vue'
// import MultiplyVue from './components/Multiply.vue';
import Multiply from './components/Multiply.vue';
import { ref } from 'vue';
const visible = ref(false)
const toggle = () => {
  visible.value = !visible.value
}
</script>

<template>
  <div class="app">
    <img src="/vite.svg" class="logo" alt="Vite logo" @click="toggle" />
    <!-- <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" /> -->
    <h1>Let's go</h1>
  </div>
  <Transition>
    <Multiply v-if="visible" />
  </Transition>
  <!-- <HelloWorld msg="Vite + Vue" /> -->
</template>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: all 0.5s ease-in-out;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
  top: -100vh;
}

.app {
  display: flex;
  width: 100vw;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  position: relative;
}

.logo {
  height: 26rem;
  padding: 1.5em;
  will-change: filter;
  cursor: pointer;
  transition: all 0.5s;
  outline: none;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
