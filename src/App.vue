<template>
  <div id="app">
    <Nav />
    <Quiz @quiz-completed="handleQuizCompleted" :key="quizKey" />
    <Modal
      v-show="showModal"
      header="Congratulations!"
      subheader="You've completed your Quiz!"
      v-bind:quizScore="quizScore"
      @reload="updateQuiz"
      @close="showModal = false"
    />
    <TutorialAd />
    <footer>
      <p id="createdBy">
        created by
        <a href="https://devdiary.me" target="_blank" rel="noopener"
          >Christian Kozalla</a
        >
      </p>
    </footer>
  </div>
</template>

<script>
import Nav from "@/components/Nav.vue";
import Quiz from "@/components/Quiz.vue";
import Modal from "@/components/Modal.vue";
import TutorialAd from "@/components/TutorialAd.vue";

export default {
  name: "App",
  components: {
    Nav,
    Quiz,
    Modal,
    TutorialAd,
  },
  data() {
    return {
      quizKey: 0,
      showModal: false,
      quizScore: {
        allQuestions: 0,
        answeredQuestions: 0,
        correctlyAnsweredQuestions: 0,
      },
    };
  },
  methods: {
    handleQuizCompleted: function(score) {
      this.quizScore = score;
      this.showModal = true;
    },
    updateQuiz: function() {
      this.showModal = false;
      this.quizKey++;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
}

#app {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

footer {
  position: fixed;
  bottom: 0;
  padding: 0.5rem 1rem;
  width: 100%;
  font-size: 0.7rem;
  background-color: rgb(102, 255, 166);
  opacity: 0.7;
}

#createdBy {
  float: right;
}
</style>
