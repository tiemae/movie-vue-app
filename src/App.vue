<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Actors</router-link><br>
      <router-link to="/movies">Movies</router-link><br>
      <router-link to="/movies/new">Add New Movie</router-link> 
    </div>
    <router-view/>
  </div>
</template>

<style>
</style>
