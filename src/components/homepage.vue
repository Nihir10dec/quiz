<template>
  <b-container>
    <h1 class="text-primary">Please select the type of Quiz You want..</h1>
    <hr />
    <b-row>
      <b-col sm="2"></b-col>
      <b-col sm="8">
        <b-form @submit="onSubmit" @reset="onReset" method="post">
          <b-form-group label="No of Questions:">
            <b-form-input
              v-model="noofquestion"
              type="number"
              required
              autofocus
              max="50"
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
      <b-col sm="2"></b-col>
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
        { value: "any", text: "Any Difficulty" },
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
    this.categories.unshift({ value: "any", text: "Any Category" });
  },
  methods: {
    async onSubmit(e) {
      e.preventDefault();

      let url =
        "https://opentdb.com/api.php?amount=" +
        this.noofquestion +
        "&encode=url3986";
      if (this.category !== "any") {
        url += "&category=" + this.category;
      }
      if (this.difficulty !== "any") {
        url += "&difficulty=" + this.difficulty.toLowerCase();
      }
      // console.log(url);

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
