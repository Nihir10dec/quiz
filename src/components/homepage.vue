<template>
  <b-container>
    <h1>Please Select What type of Quiz You want to continue with..</h1>
    <hr />
    <b-row>
      <b-col sm="9" offset="1">
        <b-form @submit="onSubmit" @reset="onReset" method="post">
          <b-form-group label="No of Questions:">
            <b-form-input
              v-model="noofquestion"
              required
              placeholder="Enter number of Questions you want..."
            ></b-form-input>
          </b-form-group>

          <b-form-group label="Category:">
            <b-form-select
              v-model="category"
              :options="categories"
              required
            ></b-form-select>
          </b-form-group>

          <b-form-group label="Difficulty:">
            <b-form-select
              v-model="difficulty"
              :options="difficulties"
              required
            ></b-form-select>
          </b-form-group>
          <br />

          <br />
          <b-button type="submit" variant="primary">Submit</b-button>&nbsp;
          <b-button type="reset" variant="danger">Reset</b-button>
        </b-form>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "homepage",
  data() {
    return {
      noofquestion: 10,
      difficulties: ["easy", "medium", "hard"],
      difficulty: null,
      categories: [],
      category: null,
      issubmitted: false,
    };
  },
  async mounted() {
    let response = await fetch("https://opentdb.com/api_category.php");
    let data = await response.json();
    this.categories = data.trivia_categories.map((a) => ({
      value: a.id,
      text: a.name,
    }));
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
        this.difficulty;

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
