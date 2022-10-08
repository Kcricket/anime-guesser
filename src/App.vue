<template>
  <div id="app">
    <h2>Drag an image of an anime</h2>
    <h4>The app will give you information about it</h4>
    <Analayzer msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Analayzer from './components/Analayzer.vue'

export default {
  name: 'App',
  components: {
    Analayzer
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
