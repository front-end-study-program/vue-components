<script setup lang="ts">
import { ref } from 'vue'
import Collapse from './packages/Collapse/index.vue'

const expanded = ref(true)

function onClick () {
  expanded.value = !expanded.value
}
</script>

<template>
  <div>
    <button @click="onClick">
      {{ expanded ? '收起' : '展开' }}
    </button>
  </div>
  <Collapse>
    <div v-if="expanded">
      <a
        href="https://vitejs.dev"
        target="_blank"
      >
        <img
          src="/vite.svg"
          class="logo"
          alt="Vite logo"
        >
      </a>
      <a
        href="https://vuejs.org/"
        target="_blank"
      >
        <img
          src="./assets/vue.svg"
          class="logo vue"
          alt="Vue logo"
        >
      </a>
    </div>
  </Collapse>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
