<template>
  <div id="app">
    <Nav />
  </div>
</template>

<script>
import Nav from "@/components/Nav.vue";
export default {
  name: "App",
  components: {
    Nav,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
