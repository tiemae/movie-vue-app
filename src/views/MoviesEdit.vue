<template>
  <div class="movies-edit">
    <h1>Edit this movie :)</h1>

      <ul>
        <li v-for="error in errors">{{ error }}</li>
      </ul>

      <form v-on:submit.prevent="submit()">
        <div class="form-row">
          <div class="form-group col-md-6">
            <label for="title">Title</label>
            <input type="text" class="form-control" id="title" placeholder="movie title" v-model="movie.title">
          </div>
          <div class="form-group col-md-6">
            <label for="year">Year</label>
            <input type="number" class="form-control" id="year" placeholder="2007" v-model="movie.year">
          </div>
        </div>
        <div class="form-group">
          <label for="plot">Plot</label>
          <input type="text" class="form-control" id="plot" placeholder="Write the movie plot here" v-model="movie.plot">
        </div>
        <div class="form-group">
          <label for="director">Director</label>
          <input type="text" class="form-control" id="director" placeholder="name of the movie director" v-model="movie.director">
        </div>
        <div class="form-group">
          <label for="english">English?</label>
          <input type="text" class="form-control" id="english" placeholder="Is this movie in english? (true or false)" v-model="movie.english">
        </div>
        <button type="submit" class="btn btn-warning">Update</button>
      </form>



  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      errors: [],
      movie: {}
    };
  },
  created: function() {
    axios.get("/api/movies/" + this.$route.params.id).then(response => {
      this.movie = response.data;
      console.log(this.movie);
    });
  },

  methods: {
    submit: function() {
      var params = {
        title: this.movie.title,
        year: this.movie.year,
        plot: this.movie.plot,
        director: this.movie.director,
        english: this.movie.english
      };
      axios.patch("/api/movies/" + this.movie.id, params).then(response => {
        console.log("Success!", response.data);
        this.$router.push("/movies/" + this.movie.id);
      }).catch(error => {
        this.errors = error.response.data.errors;
      });
    }
  }
};  
</script>