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
    <footer>
      <p id="createdBy">
        created by <a href="https://devdiary.me">Christian Kozalla</a>
      </p>
      <p id="logoReference">
        Logo made with
        <a href="https://logomakr.com" target="_blank">Logomakr.com</a>
      </p>
    </footer>
  </div>
</template>

<script>
import Nav from "@/components/Nav.vue";
import Quiz from "@/components/Quiz.vue";
import Modal from "@/components/Modal.vue";

export default {
  name: "App",
  components: {
    Nav,
    Quiz,
    Modal,
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
  font-family: Avenir, Helvetica, Arial, sans-serif;
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
}

#createdBy {
  float: right;
}

#logoReference {
  float: left;
}
</style>
