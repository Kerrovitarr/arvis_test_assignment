<template>
  <Arvis msg="Test assignment for Arvis using Vue v3"/>
</template>

<script>
import Arvis from './components/Arvis.vue'

export default {
  name: 'App',
  components: {
    Arvis
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
