<template>
  <div id="quiz-container">
    <img id="logo-crown" src="@/assets/crown.svg" alt="headsUP Crown" />
    <h1 id="logo-headline">headsUP</h1>

    <div id="correctAnswers">
      You have
      <strong>{{ correctAnswers }} correct {{ pluralizeAnswer }}!</strong>
    </div>
    <hr class="divider" />
    <div>
      <h1
        v-html="currentQuestion ? currentQuestion.question : 'Loading...'"
      ></h1>
      <form v-if="currentQuestion">
        <button
          v-for="answer in currentQuestion.answers"
          :index="currentQuestion.key"
          :key="answer"
          v-html="answer"
          @click.prevent="handleButtonClick"
        ></button>
      </form>
      <hr class="divider" />
    </div>
  </div>
</template>

<script>
export default {
  name: "Quiz",
  data() {
    return {
      questions: [],
      index: 0,
    };
  },
  computed: {
    currentQuestion: function() {
      if (this.questions !== []) {
        return this.questions[this.index];
      }
      return null;
    },
    score: function() {
      if (this.questions !== []) {
        return {
          allQuestions: this.questions.length,
          answeredQuestions: this.questions.reduce((count, currentQuestion) => {
            if (currentQuestion.userAnswer) {
              count++;
            }
            return count;
          }, 0),
          correctlyAnsweredQuestions: this.questions.reduce(
            (count, currentQuestion) => {
              if (currentQuestion.rightAnswer) {
                count++;
              }
              return count;
            },
            0
          ),
        };
      } else {
        return {
          allQuestions: 0,
          answeredQuestions: 0,
          correctlyAnsweredQuestions: 0,
        };
      }
    },

    correctAnswers: function() {
      if (this.questions && this.questions.length > 0) {
        let streakCounter = 0;
        this.questions.forEach(function(question) {
          if (!question.rightAnswer) {
            return;
          } else if (question.rightAnswer === true) {
            streakCounter++;
          }
        });
        return streakCounter;
      } else {
        return "--";
      }
    },
    pluralizeAnswer: function() {
      return this.correctAnswers === 1 ? "Answer" : "Answers";
    },
    quizCompleted: function() {
      if (this.questions.length === 0) {
        return false;
      }

      /* If a user happens to miss out on a question, the modal shall still be displayed, when the user answeres the last question! */
      let lastQuestionAnswered =
        this.questions && this.questions[this.questions.length - 1].userAnswer;

      /* Check if all questions were answered */
      let questionsAnswered = 0;
      this.questions.forEach(function(question) {
        question.rightAnswer !== null ? questionsAnswered++ : null;
      });
      return lastQuestionAnswered
        ? true
        : questionsAnswered === this.questions.length;
    },
  },
  watch: {
    quizCompleted: function(completed) {
      completed && this.$emit("quiz-completed", this.score);
    },
  },
  methods: {
    getQuestions: async function() {
      let quiz = [];
      await fetch("https://opentdb.com/api.php?amount=10&category=9")
        .then((res) => res.json())
        .then((jsonResponse) => {
          let index = 0;
          jsonResponse.results.map((question) => {
            question.answers = [
              question.correct_answer,
              ...question.incorrect_answers,
            ];
            /* Shuffle question.answers array */
            for (let i = question.answers.length - 1; i > 0; i--) {
              const j = Math.floor(Math.random() * (i + 1));
              [question.answers[i], question.answers[j]] = [
                question.answers[j],
                question.answers[i],
              ];
            }
            question.rightAnswer = null;
            question.key = index;
            index++;
          });
          quiz = jsonResponse.results;
        });
      this.questions = quiz;
    },
    handleButtonClick: function(event) {
      /* Find index to identiy question object in data */
      let index = event.target.getAttribute("index");
      let pollutedUserAnswer = event.target.innerHTML; // innerHTML is polluted with decoded HTML entities e.g ' from &#039;
      /* Clear from pollution with ' */
      let userAnswer = pollutedUserAnswer.replace(/'/, "&#039;");

      /* Set userAnswer on question object in data */
      this.questions[index].userAnswer = userAnswer;

      /* Set class "clicked" on button with userAnswer -> for CSS Styles; Disable other sibling buttons */
      event.target.classList.add("clicked");
      let allButtons = document.querySelectorAll(`[index="${index}"]`);

      for (let i = 0; i < allButtons.length; i++) {
        if (allButtons[i] === event.target) continue;

        allButtons[i].setAttribute("disabled", "");
      }

      /* Invoke checkAnswer to check Answer */
      this.checkAnswer(event, index);

      /* Set timeout and switch to next question */
      setTimeout(
        function() {
          this.index += 1;
        }.bind(this),
        2200
      );
    },
    checkAnswer: function(event, index) {
      let question = this.questions[index];

      if (
        question.userAnswer &&
        question.userAnswer === question.correct_answer
      ) {
        /* Set class on Button if user answered right, to celebrate right answer with animation joyfulButton */
        event.target.classList.add("rightAnswer");
        /* Set rightAnswer on question to true, computed property can track a streak out of 10 questions */
        this.questions[index].rightAnswer = true;
      } else {
        this.questions[index].rightAnswer = false;
        /* Show right Answer */
        let correctAnswer = this.questions[index].correct_answer;
        let allButtons = document.querySelectorAll(`[index="${index}"]`);
        allButtons.forEach(function(button) {
          if (button.innerHTML === correctAnswer) {
            button.classList.add("showRightAnswer");
          }
        });
      }
    },
  },
  mounted() {
    this.getQuestions();
  },
};
</script>

<style scoped>
#quiz-container {
  margin: 1rem auto;
  padding: 1rem;
  max-width: 750px;
}

#correctAnswers {
  text-align: center;
}

.divider {
  margin: 0.5rem 0;
  border: 3px solid rgba(102, 255, 166, 0.7);
  border-radius: 2px;
  box-shadow: 3px 5px 5px rgba(0, 0, 0, 0.3);
}

#logo-headline {
  font-size: 3rem;
  padding: 0.5rem;
  color: #f50057;
  text-align: center;
}

#logo-crown {
  display: block;
  width: 45%;
  margin: 0 auto;
}

h1 {
  font-size: 1.3rem;
  padding: 0.7rem;
}

form {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
}

button {
  font-size: 1.1rem;
  box-sizing: border-box;
  padding: 1rem;
  margin: 0.3rem;
  width: 47%;
  background: linear-gradient(
    217deg,
    rgba(0, 178, 72, 0.7),
    rgba(0, 230, 118, 0.5)
  );
  border: none;
  border-radius: 0.4rem;
  box-shadow: 3px 5px 5px rgba(0, 0, 0, 0.2);
}

button:hover:enabled {
  transform: scale(1.02);
  box-shadow: 0 3px 3px 0 rgba(0, 0, 0, 0.14), 0 1px 7px 0 rgba(0, 0, 0, 0.12),
    0 3px 1px -1px rgba(0, 0, 0, 0.2);
}

button:focus {
  outline: none;
}

button:active:enabled {
  transform: scale(1.05);
}

button.clicked {
  border: 2px dashed #ff5983;
}

@keyframes joyfulButton {
  0% {
    transform: rotate(0deg);
  }
  20% {
    transform: rotate(-40deg);
  }
  85% {
    transform: rotate(410deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes showRightAnswer {
  0% {
    opacity: 0.5;
    transform: scale(1.01);
  }
  50% {
    opacity: 0.8;
    transform: scale(1.03);
  }
  100% {
    opacity: 0.5;
    transform: scale(1.01);
  }
}

button.rightAnswer {
  animation: joyfulButton;
  animation-duration: 1500ms;
  animation-delay: 100ms;
  animation-timing-function: ease;
}

button.showRightAnswer {
  animation: showRightAnswer;
  animation-duration: 800ms;
  animation-iteration-count: 3;
  animation-timing-function: ease-in-out;
  border: 2px dashed black;
  color: black;
}
</style>
