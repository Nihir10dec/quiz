<template>
  <div id="app">
    <Header />
    <br />
    <homepage v-if="submitted === false && error===false" v-on:formsubmitted="formsubmit($event)" />
    <QuestionBox v-else-if="error===false" :questions="questions" />
    <div class="container" v-if="error">
      <h1 class="text-danger">
        Ooppss...!! Some Error Occured..
        <i class="fa fa-exclamation-triangle" aria-hidden="true"></i>
      </h1>
      <h2
        class="text-warning"
      >Please try to reduce the number of Question in this category or try some other category..</h2>
      <h2>
        <a class="btn btn-primary btn-lg" href="/">Go Back</a>
      </h2>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header";
import homepage from "./components/homepage";
import QuestionBox from "./components/QuestionBox";
// import Footer from "./components/Footer";

export default {
  name: "App",
  title: "Time For Quiz",
  components: {
    Header,
    homepage,
    QuestionBox,
  },
  data() {
    return {
      submitted: false,
      questions: [],
      error: false,
    };
  },
  methods: {
    async formsubmit(url) {
      let response = await fetch(url);
      let result = await response.json();
      // console.log(result);
      if (result.response_code === 0) {
        this.questions = result.results;
        for (let k = 0; k < this.questions.length; k++) {
          decodeURIComponent(this.questions[k].question);
          this.questions[k].question = decodeURIComponent(
            this.questions[k].question
          );
        }
        this.submitted = true;
      } else {
        this.error = true;
      }
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
