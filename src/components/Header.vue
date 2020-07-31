<template>
  <b-navbar toggleable="lg" type="dark" variant="info">
    <b-navbar-brand href="/">Time For Quiz</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav class="ml-auto">
        <a
          href="https://www.linkedin.com/in/nihir-shah/"
          target="_blank"
          class="btn btn-primary ml-2"
        >
          Linked
          <i class="fa fa-linkedin-square fa-lg" aria-hidden="true">&nbsp;</i>
        </a>
        &nbsp;&nbsp;
        <a
          href="https://github.com/Nihir10dec/quiz"
          target="_blank"
          class="btn btn-secondary mr-2"
        >
          GitHub
          <i class="fa fa-github fa-lg" aria-hidden="true">&nbsp;</i>
        </a>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script>
export default {
  name: "Header",
};
</script>

<style></style>
