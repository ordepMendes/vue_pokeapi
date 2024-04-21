<script setup>
import { onMounted, reactive, ref, computed } from "vue";
import PokemonRender from "@/components/PokemonRender.vue";
import SelectedPokemonCard from "@/components/SelectedPokemonCard.vue";
let urlBase = ref(
  "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/"
);
let pokemons = reactive(ref());
let searchPokemonField = ref("");
let pokemonSelected = reactive(ref());
let loading = ref(false);

onMounted(async () => {
  await fetch("https://pokeapi.co/api/v2/pokemon?limit=150&offset=0")
    .then((response) => response.json())
    .then((response) => (pokemons.value = response.results));
});

const pokemonsFiltered = computed(() => {
  if (pokemons.value && searchPokemonField.value) {
    return pokemons.value.filter((pokemon) => {
      if (pokemon.name.includes(searchPokemonField.value)) {
        return pokemon.name;
      }
    });
  }
  return pokemons.value;
});

const selectedPokemon = async (pokemon) => {
  loading.value = true;

  await fetch(pokemon.url)
    .then((response) => response.json())
    .then((data) => (pokemonSelected.value = data))
    .catch((err) => alert("Aconteceu algum erro! " + err))
    .finally(() => (loading.value = false));
};
</script>

<template>
  <main class="mt-5">
    <div class="container">
      <div class="row">
        <div class="col-sm-12 col-md-6">
          <SelectedPokemonCard
            :name="pokemonSelected?.name"
            :xp="pokemonSelected?.base_experience"
            :height="pokemonSelected?.height"
            :image="pokemonSelected?.sprites.other.dream_world.front_default"
            :loading="loading"
          />
        </div>
        <div class="col-sm-12 col-md-6">
          <div class="card card-list">
            <div class="card-body row">
              <div class="mb-3">
                <label for="pokemonSearch" class="form-label"
                  >Pesquise por nome ou id</label
                >
                <input
                  v-model="searchPokemonField"
                  type="text"
                  class="form-control"
                  id="pokemonSearch"
                  placeholder="ex: charmander..."
                />
              </div>
              <PokemonRender
                v-for="pokemons in pokemonsFiltered"
                :key="pokemons.name"
                :name="pokemons.name"
                :urlImage="urlBase + pokemons.url.split('/')[6] + '.svg'"
                @click="selectedPokemon(pokemons)"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style>
.card-list {
  overflow-y: scroll;
  max-height: 500px;
  overflow-x: hidden;
}
</style>