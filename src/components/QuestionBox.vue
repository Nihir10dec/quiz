<template>
  <b-container>
    <b-row>
      <b-col sm="3">
        <div>
          <p>Question Navigation</p>
          <b-avatar
            class="ml-2 mb-1"
            size="lg"
            v-for="i in questions.length"
            :key="i"
            button
            variant="info"
            @click="toogleQuestion(i)"
            :class="selectedAnswer[i - 1] !== undefined ? 'answered' : ''"
            >{{ i }}</b-avatar
          >
        </div>
      </b-col>
      <b-col sm="9">
        <b-jumbotron bg-variant="info">
          <template v-slot:lead
            >Q{{ index + 1 }} {{ questions[index].question }}</template
          >

          <hr class="my-4" />

          <b-list-group>
            <b-list-group-item
              button
              v-for="(answer, index) in answers"
              :key="answer"
              @click.prevent="selectAnswer(index)"
              :class="ansclass(index)"
              >{{ index + 1 }}.) {{ answer }}</b-list-group-item
            >
          </b-list-group>
          <br />
          <b-button
            variant="success"
            @click="prevquestion"
            :disabled="index === 0"
          >
            <b-icon icon="arrow-left-circle-fill"></b-icon>&nbsp; Prev </b-button
          >&nbsp; <b-button variant="primary">Submit</b-button>&nbsp;
          <b-button
            variant="success"
            @click="nextquestion"
            :disabled="index === questions.length - 1"
          >
            Next&nbsp;
            <b-icon icon="arrow-right-circle-fill"></b-icon>
          </b-button>
        </b-jumbotron>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "QuestionBox",
  data() {
    return {
      index: 0,
      selectedAnswer: [],
    };
  },
  props: {
    questions: Array,
  },
  methods: {
    prevquestion() {
      this.index--;
    },
    nextquestion() {
      this.index++;
    },
    selectAnswer(i) {
      this.selectedAnswer[this.index] = i;
    },
    toogleQuestion(i) {
      this.index = i - 1;
    },
    ansclass(i) {
      if (this.selectedAnswer[this.index] === i) {
        return "selected";
      }
    },
  },
  computed: {
    answers() {
      // console.log(this.questions);
      let answers = [...this.questions[this.index].incorrect_answers];
      answers.push(this.questions[this.index].correct_answer);
      console.log(this.questions[this.index].correct_answer);
      answers.sort(() => Math.random() - 0.5);
      return answers;
    },
  },
};
</script>

<style scoped>
.selected {
  background-color: lightblue;
}
.answered {
  background-color: deepskyblue;
}
</style>
