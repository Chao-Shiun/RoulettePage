<script setup>
import RouletteWheel from './components/RouletteWheel.vue'
</script>

<template>
  <div id="app">
    <h1>幸運輪盤</h1>
    <RouletteWheel />
  </div>
</template>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}

h1 {
  font-size: 2.5em;
  margin-bottom: 30px;
  color: #FFD700;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}
</style>