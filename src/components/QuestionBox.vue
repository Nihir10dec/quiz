<template>
  <b-container fluid>
    <b-row>
      <b-col sm="3">
        <div>
          <b-card-group deck>
            <b-card header="Question Navigation" header-tag="header">
              <b-list-group-item>
                <b-avatar variant="info" text=" "></b-avatar>Unattempted
                <b-avatar class="answered" text=" "></b-avatar>Attempted
              </b-list-group-item>
              <b-list-group-item v-if="quizSubmitted">
                <b-avatar class="correct" text=" "></b-avatar>Correct
                <b-avatar class="wrong" text=" "></b-avatar>Wrong
              </b-list-group-item>
              <hr />
              <b-avatar
                class="ml-2 mb-1"
                size="lg"
                v-for="i in questions.length"
                :key="i"
                button
                variant="info"
                @click="toogleQuestion(i)"
                :class="badgeClass(i)"
              >{{ i }}</b-avatar>
            </b-card>
          </b-card-group>
        </div>
      </b-col>
      <b-col sm="6">
        <b-jumbotron bg-variant="default">
          <template v-slot:lead>
            Q.{{ currentQuestion + 1 }}) &nbsp;
            {{
            decodeURIComponent(
            encodeURIComponent(questions[currentQuestion].question)
            )
            }}
          </template>

          <hr class="my-4" />

          <b-list-group>
            <b-list-group-item
              button
              v-for="(answer, index) in allanswers[currentQuestion]"
              :key="answer"
              @click.prevent="selectAnswer(index)"
              :class="ansclass(index)"
            >{{ index + 1 }}.) {{ answer }}</b-list-group-item>
          </b-list-group>
          <br />
          <b-button variant="success" @click="prevquestion" :disabled="currentQuestion === 0">
            <b-icon icon="arrow-left-circle-fill"></b-icon>&nbsp; Prev
          </b-button>&nbsp;
          <b-button variant="primary" @click="quizSUbmit" :disabled="quizSubmitted">Submit</b-button>&nbsp;
          <b-button
            variant="success"
            @click="nextquestion"
            :disabled="currentQuestion === questions.length - 1"
          >
            Next&nbsp;
            <b-icon icon="arrow-right-circle-fill"></b-icon>
          </b-button>
        </b-jumbotron>
      </b-col>
      <b-col sm="3" v-if="quizSubmitted">
        <b-card-group deck>
          <b-card header="SCORES" header-tag="header">
            <b-list-group-item variant="success">Score: {{ score }} / {{ questions.length }}</b-list-group-item>
            <b-list-group-item variant="primary">Attempted: {{ attempted }} / {{ questions.length }}</b-list-group-item>
            <b-list-group-item variant="warning">
              Accuracy:
              {{
              Number(((score / attempted) * 100).toFixed(2))
              }}%
            </b-list-group-item>
            <b-list-group-item variant="suceess" v-if="score/questions.length >= 0.75">
              Congratulation...!! You have performed very well. Keep Shining
              <i
                class="fa fa-smile-o"
                aria-hidden="true"
              ></i>
            </b-list-group-item>
            <b-list-group-item variant="suceess" v-else-if="score/attempted >= 0.75">
              That's a great accuracy...!!
              <i class="fa fa-thumbs-up" aria-hidden="true"></i>
            </b-list-group-item>
            <b-list-group-item
              variant="warning"
              v-else-if="score/questions.length >= 0.5"
            >You can perform much better...!!</b-list-group-item>
            <b-list-group-item variant="danger" v-else-if="score/questions.length < 0.5">
              That's not quite impressive..
              <i class="fa fa-frown-o" aria-hidden="true"></i>
              Please try again with lesser difficulty..
            </b-list-group-item>
          </b-card>
        </b-card-group>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "QuestionBox",
  data() {
    return {
      currentQuestion: 0,
      selectedAnswer: [],
      allanswers: [],
      quizSubmitted: false,
      questionAnswer: [],
      score: 0,
      attempted: 0,
    };
  },
  props: {
    questions: Array,
  },
  methods: {
    prevquestion() {
      this.currentQuestion--;
    },
    nextquestion() {
      this.currentQuestion++;
    },
    selectAnswer(i) {
      this.selectedAnswer[this.currentQuestion] = i;
    },
    toogleQuestion(i) {
      this.currentQuestion = i - 1;
    },
    ansclass(i) {
      let correctindex = this.allanswers[this.currentQuestion].indexOf(
        decodeURIComponent(this.questions[this.currentQuestion].correct_answer)
      );
      if (this.quizSubmitted && correctindex === i) {
        return "correct";
      }
      if (
        this.quizSubmitted &&
        correctindex !== i &&
        this.selectedAnswer[this.currentQuestion] === i
      ) {
        return "wrong";
      }
      if (
        !this.quizSubmitted &&
        this.selectedAnswer[this.currentQuestion] === i
      ) {
        return "selected";
      }
    },
    badgeClass(i) {
      if (this.quizSubmitted) {
        return this.questionAnswer[i - 1];
      } else {
        if (this.selectedAnswer[i - 1] !== undefined) {
          return "answered";
        } else return "";
      }
    },
    quizSUbmit() {
      this.quizSubmitted = true;
      for (let j = 0; j < this.questions.length; j++) {
        if (this.selectedAnswer[j] !== undefined) {
          console.log(
            this.allanswers[j][this.selectedAnswer[j]],
            decodeURIComponent(this.questions[j].correct_answer)
          );
          if (
            this.allanswers[j][this.selectedAnswer[j]] ===
            decodeURIComponent(this.questions[j].correct_answer)
          ) {
            this.questionAnswer[j] = "correct";
            this.score++;
          } else {
            this.questionAnswer[j] = "wrong";
          }
          this.attempted++;
        }
      }
    },
  },
  async mounted() {
    // console.log(this.questions);
    for (let j = 0; j < this.questions.length; j++) {
      let answers = [...this.questions[j].incorrect_answers];
      answers.push(this.questions[j].correct_answer);
      // console.log(this.questions[j].correct_answer);
      answers.sort(() => Math.random() - 0.5);
      for (let a = 0; a < answers.length; a++) {
        answers[a] = decodeURIComponent(answers[a]);
      }
      this.allanswers[j] = await answers;
    }
    // console.log(this.allanswers);
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
.correct {
  background-color: lightgreen;
}
.wrong {
  background-color: #ff4d4d;
  color: white;
}
</style>
