<template>
  <div>
    <b-jumbotron>
      <p v-html="questions.question"></p>
      <!-- <template v-slot:lead>{{ questions.question }}</template> -->

      <hr class="my-4" />

      <b-list-group class="mb-2">
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          href="#"
          @click="selectedAnswer(index)"
          :active="!answered && cindex == index"
          :variant="
           !answered && cindex === index ? 'primary' :
           answered && index === correctAnswer ? 'success' :
           answered && index === cindex && index !== correctAnswer ? 'danger' :''"
        >{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        href="#"
        @click="submitAnswer"
        class="mr-1"
        :disabled="cindex === null || answered"
      >Submit</b-button>
      <b-button variant="success" href="#" @click="next" class="mr-1" :disabled="!answered">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    questions: Object,
    next: Function,
  },
  data() {
    return {
      cindex: null,
      shuffledAnswers: [],
      correctAnswer: null,
      answered: false,
    };
  },
  methods: {
    selectedAnswer(i) {
      if(!this.answered){
        this.cindex = i;
      }
    },
    shuffleAnswers() {
      let answers = [
        ...this.questions.incorrect_answers,
        this.questions.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctAnswer = this.shuffledAnswers.indexOf(
        this.questions.correct_answer
      );
    },
    submitAnswer() {
      if (this.cindex == this.correctAnswer) {
        this.$emit("submit-answer", true);
      }
      this.answered = true;
      this.$emit("submit-answer", false);
    },
  },
  watch: {
    questions: {
      immediate: true,
      handler() {
        this.cindex = null;
        this.shuffleAnswers();
        this.answered = false;
        
      },
    },
  },
  computed: {
    answers() {
      let answers = [...this.questions.incorrect_answers];
      answers.push(this.questions.correct_answer);
      return answers;
    },
  },
};
</script>