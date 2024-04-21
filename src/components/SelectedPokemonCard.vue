<script setup>
const pokemon = defineProps(["name", "xp", "height", "image", "loading"]);
</script>

<template>
  <div class="card" :class="loading ? '' : 'animate__animated animate__flipInY'">
            <img class="card-img-top pt-2" :src="pokemon.image" :alt="pokemon.name" height="300">
            <div class="card-body">
              <h5 class="card-title text-center">
                {{pokemon.name }}
              </h5>
              <hr>
              <div class="row">
                <section class="col">
                    <strong>XP: </strong>
                    <span>{{ pokemon.xp }}</span>
                </section>
                <section class="col">
                    <strong>Altura: </strong>
                    <span>{{ pokemon.height }}</span>
                </section>
              </div>
            </div>
          </div>
</template>

<style>

</style>