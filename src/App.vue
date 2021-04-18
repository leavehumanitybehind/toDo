<template>
  <div id="app">
    <ToDo />
  </div>
</template>

<script>
import ToDo from './components/ToDo.vue'

export default {
  name: 'App',
  components: {
    ToDo
  }
}
</script>

<style>
body {
  font-family: 'Josefin Sans', sans-serif, Arial, sans-serif;
  margin: 0;
  padding: 0;
  font-size: 12px;
}



#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

}
</style>
