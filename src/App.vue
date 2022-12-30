<template>

  <ScorePoint :player="this.player" :computer="this.computer" />

  <template v-if="this.question">

    <h1 v-html="this.question"></h1>
    <template v-for="(answer, index) in this.answers" :key="index">
      <input 
      :disabled="this.isSubmited" 
      type="radio" name="options" 
      :value="answer" 
      v-model="this.chosenAnswer">

      <label v-html="answer"></label> <br>
    </template>

    <button v-if="!this.isSubmited" @click="this.submitAnswer()" class="send" type="button">ENVIAR</button>

    <template v-if="this.isSubmited">
      <div>
        <hr>
        <h3 v-if="this.chosenAnswer == this.correctAnswer">✅ RESPOSTA CORRETA!</h3>
        <h3 v-else>❌ Você errou.. a resposta correta é <u v-html="this.correctAnswer"></u></h3>
        <br>
        <button @click="this.getNewQuestion()" class="send" type="button">Próxima pergunta</button>
      </div>
    </template>

  </template>



</template>

<script>

import ScorePoint  from './components/ScorePoint.vue';

export default {
  name: 'App',

  components: {
    ScorePoint
  },

  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      isSubmited: false,
      player: 0,
      computer: 0,
      isWin: undefined,
    }
  },

  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      return answers;
    }
  },


  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        alert("Escolha uma alternativa para prosseguir!")
      } else {
        this.isSubmited = true
        if(this.chosenAnswer == this.correctAnswer){
          this.player++
        }else{
          this.computer++;
        }

      }
    },
    getNewQuestion() {
      this.chosenAnswer = undefined;
      this.isSubmited = false;

      this.axios.get('https://opentdb.com/api.php?amount=1&category=18')
        .then((response) => {
          // Pega os dados da requisicao e insere nas variaveis
          this.question = response.data.results[0].question
          this.incorrectAnswers = response.data.results[0].incorrect_answers
          this.correctAnswer = response.data.results[0].correct_answer
        });
    }

  },

  created() {
    this.getNewQuestion();
  }

}

// https://opentdb.com/api.php?amount=1&category=18

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  h1 {
    margin-top: 40px;
  }

  input[type='radio'] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;

  }
}
</style>
