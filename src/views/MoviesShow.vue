<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    this.showMovie();
  },
  methods: {
    showMovie: function () {
      axios.get(`/movies/${this.$route.params.id}`).then((response) => (this.movie = response.data));
    },
    destroyMovie: function () {
      axios.delete(`movies/${this.movie.id}`).then((response) => {
        console.log(response.data);
        this.$router.push("/movies");
      });
    },
  },
};
</script>

<template>
  <div class="show-movie">
    <h3>{{ movie.title }}</h3>
    <p>{{ movie.year }}</p>
    <p>{{ movie.plot }}</p>
    <p>{{ movie.director }}</p>
    <router-link :to="`/movies/${movie.id}/edit`">
      <button>Edit</button>
    </router-link>
    <button v-on:click="destroyMovie()">Destroy</button>
  </div>
</template>

<style></style>
