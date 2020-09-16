<template>
		<div 
      :class="{ 'question': true, 
      'correct': correct, 
      'incorrect': correct !== null && !correct,
      'missing-response': missingResponse }"
    >
      <p>{{ question.question }}</p>
      <div 
        v-for="choice in question.choices" 
        :key="question.choices.indexOf(choice)"
        :ref="'choice' + question.choices.indexOf(choice)"
        :class="{ 'question-choice': true,
                  'correct-answer': correct !== null && 
                  !correct && 
                  correctAnswer === question.choices.indexOf(choice) }"
      >
        <input 
          type="radio" 
          :name="'question' + question.id"
          v-model="questionInput"
          :value="question.choices.indexOf(choice)"
        >
        <label :for="'option' + question.choices.indexOf(choice)">{{ choice }}</label><br>
      </div>
      <div
        v-if="correct !== null"
        style="overflow: hidden; margin-top: -22px;"
      >
        <p :class="this.correct ? 'correct' : 'incorrect' ">
          {{ this.correct ? "Correct" : "Incorrect" }}
        </p>
      </div>
		</div>
</template>

<script>
  export default {
    name: 'question',
    props: [ 'question', 'correctProp', 'correctAnswerProp' ],
    data() {
      return {
        correct: null,
        correctAnswer: -1,
        questionInput: -1,
        missingResponse: false
      }
    },
    methods: {
      readSelection() {
        return this.questionInput
      },

      markAsCorrect() {
        this.correct = true
      },

      markAsIncorrect(correctAnswer) {
        this.correct = false
        // then set the correct answer to be marked here
        this.correctAnswer = correctAnswer
      },

      markAsMissingResponse(bool) {
        this.missingResponse = bool;
      }
    }
  }
</script>

<style scoped>
  .question {
    border-style: solid;
    border-width: 2px;
    border-radius: 15px;
    border-color: rgb(161, 161, 161);
    margin-bottom: 20px;
    padding-left: 10px;
    padding-right: 10px;
  }

  .question div:last-child {
    margin-bottom: 16px;
  }
  
  .question div p {
    float: right;
    margin: 0;
  }

  .question.correct {
    border-color: green;
  }

  .question.incorrect {
    border-color: red;
  }

  .question.missing-response {
    border-color: #e2e200;
  }

  p.correct {
    color: green;
  }

  p.incorrect {
    color: red;
  }

  .question-choice {
    width: 90%;
  }

  .correct-answer {
    border-style: solid;
    border-width: 1px;
    border-color: green;
    border-radius: 8px;
    background-color: #c6f7c6;
  }
</style>
