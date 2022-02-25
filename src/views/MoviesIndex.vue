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
      axios.get("/movies").then((response) => {
        this.movies = response.data;
        console.log(response.data);
      });
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
  <div class="row row-cols-1 row-cols-md-2 g-4">
    <div class="col" v-for="movie in movies" :key="movie.id">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">{{ movie.title }}</h5>
          <p class="card-text">
            {{ movie.plot }}
          </p>
          <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
            <div class="carousel-indicators">
              <button
                type="button"
                data-bs-target="#carouselExampleCaptions"
                data-bs-slide-to="0"
                class="active"
                aria-current="true"
                aria-label="Slide 1"
              ></button>
              <button
                type="button"
                data-bs-target="#carouselExampleCaptions"
                data-bs-slide-to="1"
                aria-label="Slide 2"
              ></button>
              <button
                type="button"
                data-bs-target="#carouselExampleCaptions"
                data-bs-slide-to="2"
                aria-label="Slide 3"
              ></button>
            </div>
            <div class="carousel-inner">
              <div
                class="carousel-item"
                :class="{ active: index === 0 }"
                v-for="(actor, index) in movie.actors"
                :key="actor.id"
              >
                <img :src="actor.image" class="d-block w-100" alt="..." />
                <div class="carousel-caption d-none d-md-block">
                  <h5>{{ actor.first_name + " " + actor.last_name }}</h5>
                  <p>Some representative placeholder content for the second slide.</p>
                </div>
              </div>
            </div>
            <button
              class="carousel-control-prev"
              type="button"
              data-bs-target="#carouselExampleCaptions"
              data-bs-slide="prev"
            >
              <span class="carousel-control-prev-icon" aria-hidden="true"></span>
              <span class="visually-hidden">Previous</span>
            </button>
            <button
              class="carousel-control-next"
              type="button"
              data-bs-target="#carouselExampleCaptions"
              data-bs-slide="next"
            >
              <span class="carousel-control-next-icon" aria-hidden="true"></span>
              <span class="visually-hidden">Next</span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="index-movies">
    <label for="titleFilter">Search:</label>
    <input type="text" v-model="titleFilter" list="titles" id="titleFilter" />
    <datalist id="titles">
      <option v-for="movie in movies" v-bind:key="movie.id">
        {{ movie.title }}
      </option>
    </datalist>
    <div v-for="movie in filteredMoviesByTitle" :key="movie.id">
      <h3>{{ movie.title }}</h3>
      <p>{{ movie.date }}</p>
      <p>{{ movie.plot }}</p>
      <p>{{ movie.director }}</p>
      <router-link v-bind:to="`/movies/${movie.id}`">
        <button>See info</button>
      </router-link>
    </div>
  </div>
</template>

<style></style>
