<template>
  <div class="movies-new">
    
    <h1>Add a new movie!</h1>
    
      <ul>
        <li v-for="error in errors">{{ error }}</li>
      </ul>

      <form v-on:submit.prevent="submit()">
        <div class="form-row">
          <div class="form-group col-md-6">
            <label for="title">Title</label>
            <input type="text" class="form-control" id="title" placeholder="movie title" v-model="newMovieTitle">
          </div>
          <div class="form-group col-md-6">
            <label for="year">Year</label>
            <input type="number" class="form-control" id="year" placeholder="2007" v-model="newMovieYear">
          </div>
        </div>
        <div class="form-group">
          <label for="plot">Plot</label>
          <input type="text" class="form-control" id="plot" placeholder="Write the movie plot here" v-model="newMoviePlot">
        </div>
        <div class="form-group">
          <label for="director">Director</label>
          <input type="text" class="form-control" id="director" placeholder="name of the movie director" v-model="newMovieDirector">
        </div>
        <div class="form-group">
          <label for="english">English?</label>
          <input type="text" class="form-control" id="english" placeholder="Is this movie in english? (true or false)" v-model="newMovieEnglish">
        </div>
        <button type="submit" class="btn btn-success">Create</button>
      </form>

  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      newMovieTitle: "",
      newMoviePlot: "",
      newMovieYear: "",
      newMovieDirector: "",
      newMovieEnglish: "",
      errors: []
    };
  },
  created: function() {
  },

  methods: {
    submit: function() {
      var params = {
        title: this.newMovieTitle,
        plot: this.newMoviePlot,
        year: this.newMovieYear,
        director: this.newMovieDirector,
        english: this.newMovieEnglish
      };

      axios.post("/api/movies", params).then(response => {
        this.$router.push("/movies");
      }).catch(error => {
        this.errors = error.response.data.errors;
      });

    }
  }
};  
</script>