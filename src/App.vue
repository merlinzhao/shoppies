<template>
  <div id="app">
    <Nominate />
  </div>
</template>

<script>
import Nominate from "./components/Nominate.vue";

export default {
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
  margin-top: 60px;
}
</style>
