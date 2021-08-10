<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion }}
      </template>

      <hr class="my-4" />
      {{answers}}
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click.prevent="selectAnswer(index, answer)"
          :class="{
            correct: index === selectedIndex && correctIndex === index,
            incorrect: index === selectedIndex && correctIndex !== index,
          }"
          >{{ answer }}- {{correctIndex}}</b-list-group-item
        >
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button @click="next" variant="success" href="#"> Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
  },
  data: function () {
    return {
      selectedAnswer: null,
      selectedIndex: null,
      shuffleAnswers: [],
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
    correctIndex() {
      return this.answers.indexOf(this.currentQuestion.correct_answer);
    },
  },
  watch: {
    currentQuestion() {
      this.selectedAnswer = null;
      this.shuffleAnswers();
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },

    shuffleAnswer() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.corrent_answer,
      ];
      this.shuffleAnswers = _.shuffle(answers);
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: grey;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>