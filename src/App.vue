<template>
  <div id="app">
    <Timers />
  </div>
</template>

<script>
import Timers from "./components/Timers.vue";

export default {
  name: "App",
  components: {
    Timers
  }
};
</script>

<style lang="scss">
@font-face {
  font-family: "GothamPro";
  src: local("GothamPro"), url(./assets/fonts/GothamPro.woff) format("woff");
}

body {
  background-color: $background-color;
}

#app {
  font-family: GothamPro, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: $text-color;
  margin-top: 60px;
  @include flexible();
}
</style>
