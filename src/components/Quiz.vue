<template>
    <div id="quiz">
      <h1>Quiz 1 - Geographical Trivia</h1>
      <div v-if="submitted && success" class="results">
        <b>Results</b><br>You got {{ this.correct }}/{{ this.totalQuestions }} questions correct.<br>
        <p id="percent-correct">{{ ((this.correct / this.totalQuestions) * 100) + "%" }}</p>
      </div>
      <form @submit.prevent="checkForm">
        <question 
          v-for="question in questionsAndChoices"
          :key="question.id"
          :question="question"
          :correctProp="null"
          :correctAnswerProp="-1"
        />
        <input type="submit" id="submit-button" value="Submit Quiz">
        <p 
          v-if="submitted && !success" 
          class="results error-message">
          Error - Please make sure all questions have been answered before submitting.
        </p>
      </form>
    </div>
</template>

<script>
  import Question from '@/components/Question.vue'

  export default {
    name: "quiz",
    props: [ 'questionsAndChoices' ],
    components: {
      Question,
    },
    data() {
      return {
        submitted: false,
        success: false,
        correct: null,
        totalQuestions: null
      }
    },
    methods: {
      checkForm() {
        // this is where we will error check the form, but for now
        // to save time and focus on the other stuff, we will just
        // use fake hard-coded data
        var responses = []

        var childQuestions = this.$children

        var error = false

        for (var i = 0; i < childQuestions.length; i++) {
          console.log(childQuestions[i].readSelection())

          responses.push(childQuestions[i].readSelection())

          if (childQuestions[i].readSelection() === -1) {
            childQuestions[i].markAsMissingResponse(true)
            error = true
          }
          else
            childQuestions[i].markAsMissingResponse(false)
        }

        // if responses is missing the full thing (i.e. it contains some -1)
        // then we should probably display error

        if (!error) {
          // then send up to App to evaluate
          this.$emit('submit:response', responses)
        }
        else {
          this.showError()
        }
      },

      showResults(correctArr, sumCorrect, answersArr) {
        this.correct = sumCorrect
        this.totalQuestions = correctArr.length

        this.success = true
        this.submitted = true

        var childQuestions = this.$children

        for (var i = 0; i < childQuestions.length; i++) {
          if (correctArr[i])
            childQuestions[i].markAsCorrect()
          else
            childQuestions[i].markAsIncorrect(answersArr[i])
        }

        window.scrollTo(0, 0)
      },

      showError() {
        this.submitted = true
      }
    }
  }
</script>

<style scoped>
  #quiz {
    width: 50%;
    margin-top: 40px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 80px;
    text-align: left;
  }

  h1 {
    margin-bottom: 40px;
  }

  input {
    margin-top: 20px;
  }

  .results {
    margin-bottom: 40px;
    padding-left: 10px;
  }

  #percent-correct {
    font-size: 1.4em;
    font-weight: bold;
  }

  #submit-button {
    background-color: green;
    color: white;
    text-align: center;
    margin-left: 40%;
    width: 20%;
  }

  .error-message {
    color: red;
    text-align: center;
    margin-top: 40px;
  }

  @media all and (max-width: 700px) {
    #quiz {
      width: 80%;
      margin-left: 10%;
    }

    #submit-button {
      width: 40%;
      margin-left: 30%;
    }
  }
</style>
