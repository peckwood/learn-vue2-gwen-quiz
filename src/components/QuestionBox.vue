<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          :class="answerClass(index)"
          @click="selectAnswer(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        :disabled="selectedAnswerIndex === null || submitted === true"
        @click="submitAnswer"
        >Submit</b-button
      >
      <b-button
        variant="success"
        :disabled="submitted === false"
        @click="clickNextMethod"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    clickNextMethod: Function,
    afterSubmitAnswer: Function,
  },
  data() {
    return {
      correctAnswerIndex: null,
      selectedAnswerIndex: null,
      submitted: false,
    };
  },
  methods: {
    selectAnswer(index) {
      if (!this.submitted) {
        this.selectedAnswerIndex = index;
      }
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedAnswerIndex === this.correctAnswerIndex) {
        isCorrect = true;
      }
      //method must use 'this.' as well
      this.afterSubmitAnswer(isCorrect);
      this.submitted = true;
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.submitted && this.selectedAnswerIndex === index) {
        answerClass = "selected";
      } else if (this.submitted && this.correctAnswerIndex === index) {
        answerClass = "correct";
      } else if (this.submitted && this.selectedAnswerIndex === index) {
        answerClass = "incorrect";
      }
      return answerClass;
    },
  },
  watch: {
    currentQuestion() {
      this.selectedAnswerIndex = null;
      this.submitted = false;
    },
  },
  computed: {
    answers() {
      let allAnswers = [...this.currentQuestion.incorrect_answers];
      this.correctAnswerIndex = Math.floor(Math.random() * 4);
      allAnswers.splice(
        this.correctAnswerIndex,
        0,
        this.currentQuestion.correct_answer
      );
      return allAnswers;
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover:not(.selected, .correct, .incorrect) {
  background-color: #eee;
  cursor: pointer;
}
.btn {
  margin: 0 10px;
}
.selected {
  background: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: salmon;
}
</style>