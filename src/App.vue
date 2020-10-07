<template>
  <div id="app">
    <Nav />
    <Quiz />
  </div>
</template>

<script>
import Nav from "@/components/Nav.vue";
import Quiz from "@/components/Quiz.vue";
export default {
  name: "App",
  components: {
    Nav,
    Quiz,
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
