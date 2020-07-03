<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer,index) in answers"
          :key="index"
          @click.prevent="selectedAnswer(index)"
          :class="answerClass(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>
      <br />

      <b-button
        variant="primary mr-2"
        href="#"
        @click="submitAnswers"
        :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      currectIndex: null,
      selectedIndex: null,
      shuffleAnswers: [],
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.suffleAnswer();
        this.answered = false;
      }
    }

    // (){
    //   this.selectedIndex = null;
    //   this.suffleAnswer();
    // }
  },
  methods: {
    selectedAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswers() {
      let isCorrect = false;

      if (this.selectedIndex == this.currectIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    suffleAnswer() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffleAnswers = _.shuffle(answers);
      this.currectIndex = this.shuffleAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },

    answerClass(index) {
      let answerClass = "";

      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.currectIndex === index) {
        answerClass = "correct";
      }else if(this.answered && this.selectedIndex === index && this.currectIndex !== index){
        answerClass = 'incorrect'
      }

      // [
      //   !answered && selectedIndex === index
      //     ? "selected"
      //     : answered && currectIndex === index
      //     ? "correct"
      //     : answered && selectedIndex === index && currectIndex !== index
      //     ? "incorrect"
      //     : ""
      // ];

      return answerClass;
    }
  },
  mounted: function() {
    console.log(this.currentQuestion);
  }
};
</script>

<style>
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}

.selected {
  background-color: lightblue !important;
}
.correct {
  background-color: lightgreen !important;
}
.incorrect {
  background-color: red !important;
}
</style>