<template>
  <div class="home">
    <h1>New Actor</h1>
      <div>
        First Name: <input type="text" v-model="newActorFirstName"><br>
        Last Name: <input type="text" v-model="newActorLastName"><br>
        Known For: <input type="text" v-model="newActorKnownFor"><br>
        Age: <input type="number" v-model="newActorAge"><br>
        Gender: <input type="text" v-model="newActorGender"><br>
        Movie Id: <input type="number" v-model="newActorMovieId"><br>
     </div>
    <div>
      <button v-on:click="createActor()">Create An Actor</button>
    </div>

    <h1>All Actors</h1>
    <div v-for="actor in actors">
        <h2>{{ actor.first_name }} {{ actor.last_name }}</h2><br>
        <button v-on:click="showActor(actor)">Show more</button>
        <div v-if="currentActor === actor">
          <p>Known For: {{actor.known_for}}</p>
          <p>Age: {{actor.age}} Gender: {{actor.gender}}</p><br>
          <p>Movie Id: {{actor.movie_id}} </p><br>
          <div>
            First Name: <input type="text" v-model="actor.first_name"><br>
            Last Name: <input type="text" v-model="actor.last_name"><br>
            Known For: <input type="text" v-model="actor.known_for"><br>
            Age: <input type="number" v-model="actor.age"><br>
            Gender: <input type="text" v-model="actor.gender"><br>
            Movie Id: <input type="number" v-model="actor.movie_id"><br>
            <button v-on:click="updateActor(actor)">Update Actor Information</button>
             <button v-on:click="destroyActor(actor)">Destroy Actor</button>

          </div>
        </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      actors: [],
      currentActor: {},
      newActorFirstName: "",
      newActorLastName: "",
      newActorKnownFor: "",
      newActorAge: "",
      newActorGender: "",
      newActorMovieId: ""
    };
  },
  created: function() {
    axios.get("/api/actors").then(response => {
      this.actors = response.data;
      console.log(this.actors);
    });
  },

  methods: {
    createActor: function() {
      var params = {
        first_name: this.newActorFirstName,
        last_name: this.newActorLastName,
        known_for: this.newActorKnownFor,
        age: this.newActorAge,
        gender: this.newActorGender,
        movie_id: this.newActorMovieId
      };
      axios.post("/api/actors", params).then(response => {
        console.log("success!", response.data);
        this.actors.push(response.data);
        this.newActorFirstName = "";
        this.newActorLastName = "";
        this.newActorKnownFor = "";
        this.newActorAge = "";
        this.newActorGender = "";
        this.newActorMovieId = "";
      });
    },
    showActor: function(actor) {
      if (this.currentActor === actor) {
        this.currentActor = {};
      } else {
        this.currentActor = actor;
      }
    },
    updateActor: function(actor) {
      var params = {
        first_name: actor.first_name,
        last_name: actor.last_name,
        known_for: actor.known_for,
        age: actor.age,
        gender: actor.gender,
        movie_id: actor.movie_id
      };
      axios.patch("/api/actors/" + actor.id, params).then(response => {
        console.log("Success!", response.data);
        this.currentActor = {};
      }).catch(error => {
        this.errors = error.response.data.errors;
      });
    },
    destroyActor: function(actor) {
      axios
        .delete("/api/actors/" + actor.id)
        .then(response => {
          console.log("Success!", response.data);
          var index = this.actors.indexOf(actor);
          this.actors.splice(index, 1);
        });
    }
  }
};  
</script>