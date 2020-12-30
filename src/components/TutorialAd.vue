<template>
  <div id="ad-wrapper">
    <h1>
      A Tutorial on <span class="highlight">how to build this Quiz</span> is
      availible
      <a href="https://github.com/christiankozalla/vue-quiz-tutorial" noreferrer
        >on GitHub</a
      >
      🚀
    </h1>
  </div>
</template>

<script>
export default {
  name: "TutorialAd",
};
</script>

<style scoped>
#ad-wrapper {
  padding: 0.5rem 1rem;
  text-align: center;
  background-color: rgba(245, 0, 87, 0.6);
  max-width: 730px;
  margin: 2rem auto;
  margin-bottom: 2.5rem;
  border-radius: 4px;
}

.highlight {
  font-style: italic;
  text-shadow: 1px 3px 5px rgba(54, 0, 19, 0.7);
}
</style>
