<template>
  <div id="app">
    <Resume />
  </div>
</template>

<script setup>
import Resume from './components/Resume.vue'
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: 'Microsoft YaHei', 'SimSun', 'SimHei', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: #f5f5f5;
  min-height: 100vh;
  padding: 20px;
}

@media print {
  #app {
    background: white;
    padding: 0;
  }
}
</style>
