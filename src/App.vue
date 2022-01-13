<template>
  <div id="app">
    <h1 class="title is-1">Pokedex Search</h1>
    <div class="columns is-centered">
      <div class="column is-one-quarter">
        <input
          type="text"
          class="input is-info is-rounded"
          v-model="busca"
          placeholder="buscar pokemon pelo nome"
        />
      </div>
    </div>

    <div id="pokemons">
        <div style='width: 740px; column-count:2; column-rule-style: dashed;'>
          <div v-for="(poke, index) in resultadoBusca" :key="index">
            <Pokemon :name="poke.name" :url="poke.url" :num="index" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("http://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  computed: {
    resultadoBusca: function () {
      if (this.busca == "" || this.busca == " ") {
        return this.pokemons;
      } else {
        return this.pokemons.filter((pokemon) => pokemon.name == this.busca);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-bottom: 60px;
}

#pokemons {
    display: flex;
    flex-direction: column;
    align-items: center;
}
</style>
