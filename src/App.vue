<template>

  <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount" />

  <div v-if="this.question">
    <h1 v-html="this.question"></h1>
    <template v-for="(answer, index) in this.answers" :key="index">
      <input 
        type="radio"
        name="options"
        :value="answer"
        v-model="this.chosenAnswer" 
        :disabled="this.answerSubmitted"
      >
      <label for="options" v-html="answer"></label>
      <br>
    </template>

    <button v-if="!this.answerSubmitted" class="send" type="button" @click="submitAnswer()">Send</button>

    <section class="result" v-if="this.answerSubmitted">
      <h4 v-if="this.chosenAnswer == this.correctAnswer">
        &#9989; Congratulations, the answer "{{ this.correctAnswer }}" is correct!
      </h4>
      <h4 v-else>
        &#10060; Sorry, your answer is incorrect, the correct answer is "{{ this.correctAnswer }}"
      </h4>
      <button @click="getNewQuestion()" type="button" class="next-question">Next Question</button>
    </section>
  </div>
</template>

<script>
const API = "https://opentdb.com/api.php?amount=1";
import ScoreBoard from ".//components/ScoreBoard.vue";

export default {
  name: 'App',

  components: {
    ScoreBoard
  },

  methods: {
    submitAnswer() {
      if(!this.chosenAnswer){
        alert("Please Pick one answer")
      }else{
        this.answerSubmitted = true;
        if (this.chosenAnswer == this.correctAnswer){
          this.winCount++
        }else{
          this.loseCount++
        }
      }
    },
    getNewQuestion(){
      this.answerSubmitted = false
      this.question = undefined
      this.chosenAnswer = undefined

      this.axios.get(API).then((response) => {
        // console.log(response.data.results)
        var questionResults = response.data.results;
        this.question = questionResults[0].question;
        this.correctAnswer = questionResults[0].correct_answer;
        this.inCorrectAnswers = questionResults[0].incorrect_answers;
      })
    }
  },
  data(){
    return{
      question: undefined,
      correctAnswer: undefined,
      inCorrectAnswers: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0
    }
  },

  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.inCorrectAnswers))
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer)

      return answers;
    }
  },

  created() {
    this.getNewQuestion()
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

  button.send, button.next-question{
    appearance: none;
    border: none;
    border-radius: 10px;
    background-color: #0101ef;
    color: #fff;
    font-size: 20px;
    padding: 15px 45px;
    margin-top: 20px;
    cursor: pointer;
    // text-transform: uppercase;
  }

  input, label{
    font-size: 18px;
  }
}
</style>
