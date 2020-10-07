<template>
  <div id="quiz-container">
    <h1 id="quiz-headline">headsUP Quiz</h1>
    <div id="streak">
      Your current Streak is {{ streak }} correct {{ pluralizeAnswer }}
    </div>
    <hr class="divider" />
    <div v-for="question in questions" :key="question.key">
      <h1 v-html="question.question"></h1>
      <form>
        <button
          v-for="answer in question.answers"
          :index="question.key"
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
    };
  },
  computed: {
    streak: function() {
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
      return this.streak === 1 ? "Answer" : "Answers";
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
      let userAnswer = event.target.innerHTML;

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

#quiz-headline {
  font-size: 2rem;
  text-align: center;
  margin-bottom: 0.5rem;
}

#streak {
  text-align: center;
}

.divider {
  margin: 0.5rem 0;
  border: 3px solid rgba(102, 255, 166, 0.7);
  border-radius: 2px;
  box-shadow: 3px 5px 5px rgba(0, 0, 0, 0.3);
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
  transform: scale(1.04);
  border: 2px dashed #ff5983;
}

@keyframes joyfulButton {
  0% {
    transform: rotate(0deg);
  }
  20% {
    transform: rotate(-40deg);
  }
  70% {
    transform: rotate(390deg);
  }
  100% {
    transform: rotate(0deg);
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
  animation-duration: 2000ms;
  animation-delay: 200ms;
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
