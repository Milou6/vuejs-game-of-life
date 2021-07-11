<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
  <div>
    <h1>Vuejs Game of life</h1>
    <Grid msg="My message prop" :lengthX="15" :lengthY="35" />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue';
import Grid from './components/Grid.vue';

export default {
  name: 'App',
  components: {
    // HelloWorld,
    Grid,
  },
};
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
