<template>
  <div id="app">
    <div class="flex flex-col items-center justify-center mb-20">
      <img src="./assets/pokemon_logo.png" alt="Pokemon Logo" />
      <div class="mt-10">
        <input
          type="text"
          placeholder="Buscar Pokémon (letra minuscula)"
          class="border border-gray-600 rounded px-1 mr-2 w-64"
          v-model="search"
          @change="searchPokemons"
        />
        <button
          id="searchBtn"
          class="bg-green-400 border rounded w-24 text-slate-50"
          @click="searchPokemons"
        >
          Buscar
        </button>
      </div>
    </div>
    <div>
      <div id="pokemon-list" class="flex items-center justify-center flex-wrap">
        <div :key="pokemon.url" v-for="(pokemon, index) in filteredPokemons">
          <pokemon :index="index + 1" :name="pokemon.name" :url="pokemon.url" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import api from "./services/api";
import pokemon from "./components/pokemon.vue";
export default {
  name: "App",
  components: {
    pokemon,
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      search: "",
      emptySearch: "",
    };
  },
  // Função é executada quando a página é criada
  created: function () {
    api.get("pokemon?limit=151&offset=0").then((response) => {
      this.pokemons = response.data.results;
      this.filteredPokemons = response.data.results;
    });
  },
  methods: {
    searchPokemons: function () {
      this.filteredPokemons = this.pokemons;
      if (this.search == "" || this.search == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name == this.search
        );
      }
    },
  },
};
</script>

<!-- <style>
#pokemon-list {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}
</style> -->