<template>
  <div>
    <Header :correctCount="correctCount" :totalCount="questionList.length" />

    <b-container>
      <b-row>
        <b-col cols="6" offset="3" class="pt-2">
          <Content
            v-if="questionList.length"
            :questions="questionList[index]"
            :next="next"
            @submit-answer="increament($event)"
            @back-question="backQuestion"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Content from "./components/Content.vue";

export default {
  name: "App",
  components: {
    Header,
    Content,
  },
  data() {
    return {
      questionList: [],
      index: 0,
      correctCount: 0,
      totalCount: 0,
    };
  },
  methods: {
    next() {
      if (this.index < 9) {
        this.index++;
      }
    },
    increament(is_correct) {
      if (is_correct) {
        this.correctCount++;
      }
    },
  },
  mounted: async function () {
    let questionList = await fetch(
      "https://opentdb.com/api.php?amount=10&difficulty=easy&type=multiple",
      { method: "get" }
    )
      .then((res) => res.json())
      .then((data) => {
        return data.results;
      });
    this.questionList = questionList;
  },
};
</script>