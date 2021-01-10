<template>
  <div id="app">
    <link
      rel="stylesheet"
      href="https://use.fontawesome.com/releases/v5.2.0/css/all.css"
      integrity="sha384-hWVjflwFxL6sNzntih27bfxkr27PmbbK/iSvJ+a4+0owXq79v+lsFkW54bOGbiDQ"
      crossorigin="anonymous"
    />
    <Banner />
    <Nominate />
  </div>
</template>


<script>
import Nominate from "./components/Nominate.vue";
import Banner from "./components/Banner.vue";

export default {
  //https://shoppiesbymerlin.netlify.app
  name: "Shoppies",
  components: {
    Nominate,
    Banner,
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
