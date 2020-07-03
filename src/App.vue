<template>
  <div>
    <Header 
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <question
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Question from "./components/Question.vue";

export default {
  name: "App",
  components: {
    Header,
    Question
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
     if (isCorrect) {
       this.numCorrect ++;
     }
     this.numTotal++;
    }
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal:0
    };
  },

  mounted: function() {
    fetch(
      "https://opentdb.com/api.php?amount=10&category=18&difficulty=medium&type=multiple",
      {
        method: "get"
      }
    )
      .then(response => response.json())
      .then(res => {
        this.questions = res.results;
      });
  }
};
</script>

<style>
</style>
