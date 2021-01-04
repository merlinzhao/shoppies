<template>
  <div id="app">
    <Nominate />
  </div>
</template>


<script>
import Nominate from "./components/Nominate.vue";

export default {
  //https://shoppiesbymerlin.netlify.app
  name: "Shoppies",
  components: {
    Nominate,
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
}
body {
  margin: 0;
  background: #222;
}
</style>
