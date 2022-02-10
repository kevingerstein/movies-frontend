<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
      newMovie: {},
      currentMovie: {},
      editMovieParams: {},
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies").then((response) => (this.movies = response.data));
    },
    createMovie: function () {
      axios
        .post("http://localhost:3000/movies", this.newMovie)
        .then((response) => {
          this.movies.push(response.data);
          this.newMovie = {};
        })
        .catch((error) => console.log(error.response.data.errors));
    },
    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      this.editMovieParams = movie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function (movie) {
      axios
        .patch(`http://localhost:3000/movies/${movie.id}`, movie)
        .then((response) => console.log(response.data))
        .catch((error) => console.log(error.response.data.errors));
    },
    destroyMovie: function (movie) {
      if (confirm("Are you sure you to delete this?")) {
        axios
          .delete(`http://localhost:3000/movies/${movie.id}`)
          .then(() => {
            let index = this.movies.indexOf(movie);
            this.movies.splice(index, 1);
          })
          .catch((error) => console.log(error.response.data.errors));
      }
    },
  },
};
</script>

<template>
  <div class="home">
    <div v-for="movie in movies" :key="movie.id">
      <h3>{{ movie.title }}</h3>
      <button v-on:click="showMovie(movie)">See info</button>
    </div>
    <dialog id="movie-details">
      <form method="dialog">
        <h2>Movie Info</h2>
        <p>
          Title:
          <input type="text" v-model="editMovieParams.title" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="editMovieParams.plot" />
        </p>
        <p>
          Director:
          <input type="text" v-model="editMovieParams.director" />
        </p>
        <p>
          Year:
          <input type="text" v-model="editMovieParams.year" />
        </p>
        <p>
          In English:
          <input type="checkbox" v-model="editMovieParams.english" id="english" />
        </p>
        <button v-on:click="updateMovie(editMovieParams)">Update</button>
        <button v-on:click="destroyMovie(editMovieParams)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
    <div>
      <h2>Add a new Movie below...</h2>
      <p>
        Title:
        <input type="text" v-model="newMovie.title" id="title" />
      </p>
      <p>
        Year:
        <input type="number" v-model="newMovie.year" min="1800" max="2022" id="year" />
      </p>
      <p>
        Plot:
        <input type="text" v-model="newMovie.plot" id="plot" />
      </p>
      <p>
        Director:
        <input type="text" v-model="newMovie.director" id="director" />
      </p>
      <p>
        In English:
        <input type="checkbox" v-model="newMovie.english" id="english" />
      </p>
      <button v-on:click="createMovie()">Add New Movie</button>
    </div>
  </div>
</template>

<style></style>
