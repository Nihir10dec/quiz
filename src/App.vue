<template>
  <div id="app">
    <Header />
    <br />
    <homepage
      v-if="submitted === false"
      v-on:formsubmitted="formsubmit($event)"
    />
    <QuestionBox v-else :questions="questions" />
  </div>
</template>

<script>
import Header from "./components/Header";
import homepage from "./components/homepage";
import QuestionBox from "./components/QuestionBox";
// import Footer from "./components/Footer";

export default {
  name: "App",
  components: {
    Header,
    homepage,
    QuestionBox,
  },
  data() {
    return {
      submitted: false,
      questions: [],
    };
  },
  methods: {
    async formsubmit(url) {
      let response = await fetch(url);
      let result = await response.json();
      console.log(result);
      this.questions = result.results;
      for (let k = 0; k < this.questions.length; k++) {
        decodeURIComponent(this.questions[k].question);
        this.questions[k].question = decodeURIComponent(
          this.questions[k].question
        );
      }
      this.submitted = true;
    },
  },
};
</script>

<style>
html,
body {
  min-height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 6px;
  margin-bottom: 6px;
  display: flex;
  flex-direction: column;
  height: 100%;
}
</style>
