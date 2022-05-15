<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>

  <div v-else>
    <h1>¿Quién es este pokémon?</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />

    <PokemonOptions :pokemons="pokemonArr" @selection="chackAnswer" />

    <template v-if="showAnser">
      <h2>{{ message }}</h2>
      <button @click="newGame">Nuevo juego</button>
    </template>
  </div>
</template>

<script>
import PokemonPicture from "@/components/PokemonPicture";
import PokemonOptions from "@/components/PokemonOptions";

import getPokemonOptions from "@/helpers/getPokemonsOptions";

export default {
  components: {
    PokemonPicture,
    PokemonOptions,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnser: false,
      message: "",
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();

      const rnInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonArr[rnInt];
    },
    chackAnswer(pokemonId) {
      this.showPokemon = true;
      this.showAnser = true;
      if (pokemonId === this.pokemon.id) {
        this.message = `Correcto, ${this.pokemon.name}`;
      } else {
        this.message = `Oops, era ${this.pokemon.name}`;
      }
    },
    newGame() {
      this.showPokemon = false;
      this.showAnser = false;
      this.pokemonArr = [];
      this.pokemon = null;
      this.mixPokemonArray();
    },
  },
  mounted() {
    this.mixPokemonArray();
  },
};
</script>

<style></style>
