<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movieParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/movies/${this.$route.params.id}`).then((response) => {
      this.movieParams = response.data;
    });
  },
  methods: {
    updateMovie: function () {
      axios
        .patch(`/movies/${this.movieParams.id}`, this.movieParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/movies/${this.movieParams.id}`);
          this.movieParams = {};
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
  <div class="update-movie">
    <ul>
      <li v-for="error in errors" :key="error.id">{{ error }}</li>
    </ul>
    <form v-on:submit.prevent="updateMovie()">
      <p>
        <label for="title">Title:</label>
        <input type="text" id="title" v-model="movieParams.title" />
      </p>
      <p>
        <label for="year">Year:</label>
        <input type="text" id="text" v-model="movieParams.year" />
      </p>
      <p>
        <label for="year">Year:</label>
        <input type="date" id="date" v-model="movieParams.date" />
      </p>
      <p>
        <label for="plot">Plot:</label>
        <textarea id="plot" v-model="movieParams.plot"></textarea>
      </p>
      <p>
        <label for="director">Director:</label>
        <input type="text" id="director" v-model="movieParams.director" />
      </p>
      <p>
        <label for="english">English:</label>
        <input type="checkbox" id="english" v-model="movieParams.english" />
      </p>
      <input type="submit" value="Update" />
    </form>
  </div>
</template>

<style></style>
