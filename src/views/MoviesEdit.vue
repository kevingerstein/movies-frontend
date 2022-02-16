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
  <div class="create-movie">
    <ul>
      <li v-for="error in errors" :key="error.id">{{ error }}</li>
    </ul>
    <form v-on:submit.prevent="updateMovie()">
      <p>
        Title:
        <input type="text" v-model="movieParams.title" />
      </p>
      <p>
        Year:
        <input type="text" v-model="movieParams.year" />
      </p>
      <p>
        Plot:
        <input type="text" v-model="movieParams.plot" />
      </p>
      <p>
        Director:
        <input type="text" v-model="movieParams.director" />
      </p>
      <p>
        English:
        <input type="checkbox" v-model="movieParams.english" />
      </p>
      <input type="submit" value="Update" />
    </form>
  </div>
</template>

<style></style>
