<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movieParams: {},
      errors: [],
    };
  },
  methods: {
    createMovie: function () {
      axios
        .post("/movies", this.movieParams)
        .then((response) => {
          console.log(response.data);
          this.movieParams = {};
          this.$router.push("/movies");
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="create-movie">
    <ul>
      <li v-for="error in errors" :key="error.id">{{ error }}</li>
    </ul>
    <form v-on:submit.prevent="createMovie()">
      <p>
        <label for="title">Title:</label>
        <input type="text" id="title" v-model="movieParams.title" />
      </p>
      <p>
        <label for="year">Year:</label>
        <input type="text" id="year" v-model="movieParams.year" />
      </p>
      <p>
        <label for="date">Date:</label>
        <input type="date" id="date" v-model="movieParams.year" />
      </p>
      <p>
        <label for="plot">Plot:</label>
        <input type="text" id="plot" v-model="movieParams.plot" />
      </p>
      <p>
        <label for="director">Director:</label>
        <input type="text" id="director" v-model="movieParams.director" />
      </p>
      <p>
        <label for="english">English:</label>
        <input type="checkbox" id="english" v-model="movieParams.english" />
      </p>
      <input type="submit" value="Create" />
    </form>
  </div>
</template>

<style></style>
