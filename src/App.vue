<template>
  <div id="app">
    <header id="title">Quiz Application</header>
    <quiz 
      ref="quiz"
      :questionsAndChoices="questionsAndChoices"
      @submit:response="submitResponse"
    />
  </div>
</template>

<script>
  import json from '@/data/questions.json'

  import Quiz from '@/components/Quiz.vue'

  export default {
    name: 'app',
    components: {
      Quiz
    },
    data() {
      return {
        sumCorrect: null,
      }
    },
    computed: {
      questionsAndChoices() {
        var questionsAndChoices = JSON.parse(JSON.stringify(json.questions));

        for (var i = 0; i < questionsAndChoices.length; i++) {
          delete questionsAndChoices[i].answer
        }

        return questionsAndChoices
      }
    },
    methods: {
      submitResponse(responseArr) {
        var sumCorrect = 0
        var correct = []
        var answersArr = []

        for (var i = 0; i < json.questions.length; i++) {
          answersArr.push(json.questions[i].answer)
        }

        console.log(responseArr)
        

        for (i = 0; i < responseArr.length; i++) {
          correct.push(responseArr[i] === answersArr[i])
          if (responseArr[i] === answersArr[i])
            sumCorrect++
        }

        this.sumCorrect = sumCorrect

        this.$refs.quiz.showResults(correct, sumCorrect, answersArr);
      }
    }
  }
</script>

<style>
  body {
    margin: 0;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  #title {
    font-size: 2em;
    text-align: left;
    padding: 7px;
    background-color: red;
    color: white;
  }
</style>
