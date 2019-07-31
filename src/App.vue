<template>
  <div id="app">
    <Signup msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Signup from './components/Signup.vue'

export default {
  name: 'app',
  components: {
    Signup
  }
}
</script>

<style>
body, html {
  margin: 0;
  padding: 0;
  
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-top: 60px;
  /* background-color: #64cd8e; */
  background-color: #141627;
  min-width: 100%;
  min-height: 100vh;
}
</style>
