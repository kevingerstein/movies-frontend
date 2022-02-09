<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Movies!",
      movies: [],
      newMovie: {},
      movieInfo: {},
      id: null,
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      this.movies.push = axios.get("http://localhost:3000/movies").then((response) => (this.movies = response.data));
    },
    createMovie: function () {
      axios.post("http://localhost:3000/movies", this.newMovie).then((response) => {
        this.movies.push(response.data);
        this.newMovie = {};
      });
    },
    showMovie: function (id) {
      axios.get(`http://localhost:3000/movies/${id}`).then((response) => {
        this.movieInfo = response.data;
        this.id = this.movieInfo.id;
      });
    },
    updateMovie: function (id) {
      axios
        .patch(`http://localhost:3000/movies/${id}`, this.movieInfo)
        .then((response) => {
          this.movies.forEach((movie, index) => {
            if (movie.id == response.data.id) {
              movie = response.data;
              this.movies[index] = response.data;
            }
          });
        })
        .catch((error) => console.log(error.response));
    },
    destroyMovie: function (id) {
      axios.delete(`http://localhost:3000/movies/${id}`).then(() => {
        this.movies = this.movies.filter((movie) => movie.id !== id);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="movie in movies" :key="movie.id">
      <h3>{{ movie.title }}</h3>
      <button v-on:click="showMovie(movie.id)">See info</button>
      <button v-on:click="destroyMovie(movie.id)">Delete Movie</button>
      <div v-if="movie.id === movieInfo.id">
        Movie year: {{ movieInfo.year }}
        <br />
        Movie director: {{ movieInfo.director }}
        <br />
        Movie plot: {{ movieInfo.plot }} Movie
        <br />
        <div>
          <h3>Update below...</h3>
          <label for="title">Title:</label>
          <input type="text" v-model="movieInfo.title" id="title" />
          <br />
          <label for="title">Year:</label>
          <input type="number" v-model="movieInfo.year" min="1800" max="2022" id="year" />
          <br />
          <label for="title">Plot:</label>
          <input type="text" v-model="movieInfo.plot" id="plot" />
          <br />
          <label for="title">Director:</label>
          <input type="text" v-model="movieInfo.director" id="director" />
          <br />
          <label for="title">English?</label>
          <input type="checkbox" v-model="movieInfo.english" id="english" />
          <br />
          <button v-on:click="updateMovie(id)">Update Movie</button>
        </div>
      </div>
    </div>
    <div>
      <h2>Add a new Movie below...</h2>
      <label for="title">Title:</label>
      <input type="text" v-model="newMovie.title" id="title" />
      <br />
      <label for="title">Year:</label>
      <input type="number" v-model="newMovie.year" min="1800" max="2022" id="year" />
      <br />
      <label for="title">Plot:</label>
      <input type="text" v-model="newMovie.plot" id="plot" />
      <br />
      <label for="title">Director:</label>
      <input type="text" v-model="newMovie.director" id="director" />
      <br />
      <label for="title">English?</label>
      <input type="checkbox" v-model="newMovie.english" id="english" />
      <br />
      <button v-on:click="createMovie()">Add New Movie</button>
    </div>
  </div>
</template>

<style></style>
