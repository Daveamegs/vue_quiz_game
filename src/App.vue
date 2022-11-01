<template>
  <div>
    <h1 v-html="this.question"></h1>

  </div>
</template>

<script>
const API = "https://opentdb.com/api.php?amount=1";

export default {
  name: 'App',
  methods: {
    getQuiz() {}
  },
  data(){
    return{
      question: undefined,
      correctAnswer: undefined,
      inCorrectAnswers: undefined
    }
  },

  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.inCorrectAnswers))
      answers.push(this.correctAnswer)

      return answers;
    }
  },

  created() {
    this.axios.get(API).then((response) => {
      // console.log(response.data.results)
      var questionResults = response.data.results;
      this.question = questionResults[0].question;
      this.correctAnswer = questionResults[0].correct_answer;
      this.inCorrectAnswers = questionResults[0].incorrect_answers;
    })
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
