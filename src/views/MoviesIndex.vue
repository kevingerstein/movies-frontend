<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
      titleFilter: "",
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
  computed: {
    filteredMoviesByTitle() {
      return this.movies.filter((movie) => {
        return movie.title.toLowerCase().includes(this.titleFilter.toLowerCase());
      });
    },
  },
};
</script>

<template>
  <div class="index-movies">
    <input type="text" v-model="titleFilter" list="titles" />
    <datalist id="titles">
      <option v-for="movie in movies" v-bind:key="movie.id">
        {{ movie.title }}
      </option>
    </datalist>
    <div v-for="movie in filteredMoviesByTitle" :key="movie.id">
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
