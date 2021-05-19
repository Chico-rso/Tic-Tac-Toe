<template>
  <div id="app">
    <div id="details">
      <h1>Tic Tac Toe</h1>
    </div>
    <grid></grid>
  </div>
</template>

<script>
import Grid from "./components/Grid.vue";
export default {
  name: "App",
  components: { Grid },
  data() {
    return {
      matches: 0,
      wins: {
        0: 0,
        X: 0,
      },
    };
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
