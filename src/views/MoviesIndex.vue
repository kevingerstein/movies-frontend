<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/movies").then((response) => (this.movies = response.data));
    },
  },
};
</script>

<template>
  <div class="index-movies">
    <div v-for="movie in movies" :key="movie.id">
      <h3>{{ movie.title }}</h3>
      <p>{{ movie.year }}</p>
      <p>{{ movie.plot }}</p>
      <p>{{ movie.director }}</p>
      <router-link v-bind:to="`/movies/${movie.id}`">
        <button>See info</button>
      </router-link>
    </div>
  </div>
</template>

<style></style>
