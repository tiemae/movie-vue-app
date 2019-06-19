<template>
  <div class="movies-index">
  
    <h1>All Movies</h1>
    <div v-for="movie in movies">
        <h2>{{ movie.title }}</h2><br>

    <router-link v-bind:to="'/movies/' + movie.id">
    <button>More Info</button>
    </router-link>
    </div>

  </div>
</template>


<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movies: [],
      movie: {},
      errors: []
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      this.movies = response.data;
      console.log(response.data);
    }).catch(error => {
      this.errors = error.response.data.errors;
    });
  },
};
</script>
