<script setup>
import { onMounted, reactive, ref, computed } from "vue";
import PokemonRender from "@/components/PokemonRender.vue";
let urlBase = ref(
  "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/"
);
let pokemons = reactive(ref());
let searchPokemonField = ref("");

onMounted(() => {
  fetch("https://pokeapi.co/api/v2/pokemon?limit=150&offset=0")
    .then((response) => response.json())
    .then((response) => (pokemons.value = response.results));
});

const pokemonsFiltered = computed(() => {
  if(pokemons.value && searchPokemonField.value){
    return pokemons.value.filter(pokemon => {
      if(pokemon.name.includes(searchPokemonField.value)){
        return pokemon.name
      }
    })
  }
  return pokemons.value
})
</script>

<template>
  <main class="mt-5">
    <div class="container">
      <div class="row">
        <div class="col-sm-12 col-md-6">
          <div class="card">
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
              />
            </div>
          </div>
        </div>
        <div class="col-sm-12 col-md-6">Column</div>
      </div>
    </div>
  </main>
</template>
