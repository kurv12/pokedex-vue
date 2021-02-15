<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img id="logo" src="./assets/mega.png" />
      <hr />
      <h4 class="is-size-4">Pokedex</h4>
      <input
        class="input is-rounded"
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="busca"
      />
      <button
        class="button is-fullwidth is-success"
        id="buscabtn"
        @click="buscar"
      >
        Buscar
      </button>
      <div v-for="(poke, index) in filteresPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
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
      filteresPokemons: [],
      busca: "",
    };
  },
  created: function() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Pega uma lista de pokemon");
        this.pokemons = res.data.results;
        this.filteresPokemons = res.data.results;
      });
  },
  methods: {
    buscar: function() {
      this.filteresPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteresPokemons = this.pokemons;
      } else {
        this.filteresPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name == this.busca
        );
      }
    },
  },
  components: {
    Pokemon,
  },
  // computed: {
  //   resultadoBusca: function() {
  //     if (this.busca == "" || this.busca == " ") {
  //       return this.pokemons;
  //     } else {
  //       return this.pokemons.filter((pokemon) => pokemon.name == this.busca);
  //     }
  //   },
  // },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#logo {
  width: 300px;
}

#buscabtn {
  margin-top: 2%;
}
</style>
