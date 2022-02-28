<template>
  <div id="app">
    <Header 
    :correctAnswerCount="correctAnswerCount"
    :totalAnswerCount="totalAnswerCount"
    />
    <!-- v-if="questions.length", not v-if="questions" -->
    <b-container v-if="questions.length" class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            :currentQuestion="questions[index]"
            :clickNextMethod="getNext"
            :afterSubmitAnswer="afterSubmitAnswer"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctAnswerCount: 0,
      totalAnswerCount: 0
    };
  },
  methods: {
    getNext() {
      this.index++;
    },
    afterSubmitAnswer(isCorrect){
      if(isCorrect){
        this.correctAnswerCount++;
      }
      this.totalAnswerCount++;
    }
  },
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "get",
    })
      .then((response) => {
        console.log(response);
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
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
  margin-top: 60px;
}
</style>
