<template>
  <b-container>
    <h1 class="text-primary">Please Select What type of Quiz You want to continue with..</h1>
    <hr />
    <b-row>
      <b-col sm="1"></b-col>
      <b-col sm="10">
        <b-form @submit="onSubmit" @reset="onReset" method="post">
          <b-form-group label="No of Questions:">
            <b-form-input
              v-model="noofquestion"
              type="number"
              required
              autofocus
              placeholder="Enter number of Questions you want..."
            ></b-form-input>
          </b-form-group>

          <b-form-group label="Category:">
            <b-form-select v-model="category" :options="categories" required></b-form-select>
          </b-form-group>

          <b-form-group label="Difficulty:">
            <b-form-select v-model="difficulty" :options="difficulties" required></b-form-select>
          </b-form-group>
          <br />

          <b-button type="submit" variant="primary">Submit</b-button>&nbsp;
          <b-button type="reset" variant="danger">Reset</b-button>
        </b-form>
      </b-col>
      <b-col sm="1"></b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "homepage",
  data() {
    return {
      noofquestion: 10,
      difficulties: [
        { value: null, text: "Please select an option" },
        "Easy",
        "Medium",
        "Hard",
      ],
      difficulty: null,
      categories: [],
      category: null,
      issubmitted: false,
    };
  },
  async mounted() {
    let response = await fetch("https://opentdb.com/api_category.php");
    let data = await response.json();
    data.trivia_categories.sort((a, b) => (a.name > b.name ? 1 : -1));
    this.categories = data.trivia_categories.map((a) => ({
      value: a.id,
      text: a.name,
    }));
    this.categories.unshift({ value: null, text: "Please select an option" });
  },
  methods: {
    async onSubmit(e) {
      e.preventDefault();

      let url =
        "https://opentdb.com/api.php?amount=" +
        this.noofquestion +
        "&category=" +
        this.category +
        "&difficulty=" +
        this.difficulty.toLowerCase() +
        "&encode=url3986";

      this.$emit("formsubmitted", url);
    },
    onReset() {
      this.noofquestion = 10;
      this.difficulty = null;
      this.category = null;
    },
  },
};
</script>

<style></style>
