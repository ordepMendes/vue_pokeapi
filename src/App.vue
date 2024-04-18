<script setup>
import { ref } from "vue";
let num = ref(0);
</script>


<template>
  <nav class="navbar bg-dark navbar-dark">
    <div class="container-fluid">
      <router-link to="/" class="navbar-brand"><img src="./assets/images/logo-header.png" width="25px"/> Pokédex </router-link>
    </div>
  </nav>
  <div class="main">
    <router-view></router-view>
  </div>
</template>