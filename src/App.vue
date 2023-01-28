<script setup>
import { ref } from 'vue'

let num = ref(0)

</script>


<template>
  <!-- NAVBAR -->
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">
        <img src="./assets/icons/pokeball-pokemon.svg" width="30" height="24" class="d-inline-block align-text-top">
        PokeVite
      </router-link>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link" aria-current="page" to="/">Home</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/about">About</router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- CONTENT -->
  <div class="main">
    <router-view></router-view>
  </div>


  <footer class="bg-primary text-light">
    <div>Designed in {{ new Date().getFullYear() }}</div>
  </footer>

</template>

<style >
body{
  background-attachment: fixed;
  height: 100%;
  background: rgb(236,104,242);
  background: linear-gradient(0deg, rgba(236,104,242,1) 0%, rgba(34,13,224,1) 100%);
}

footer{
  position:fixed;
  bottom: 0;
  width: 100%;
  display:flex;
  align-items:center;
  justify-content:center;
  height: 56px;
}
</style>