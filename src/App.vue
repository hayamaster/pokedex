<template>
  <div>
    <header class="header">
      <label>
        Type pokemon name or ID:
        <input
          type="text"
          v-model="pokemonID"
          placeholder="enter pokemon ID!"
        />
        <button class="searchBtn" @click="searchPokemon">
          Search pokemon!
        </button>
      </label>
    </header>

    <main class="main" v-if="Object.entries(pokemonData).length > 0">
      <section class="pokemonCard">
        <div class="nameImage">
          <h1 class="pokemonName">{{ pokemonData.name }}</h1>
          <img
            :src="pokemonData.sprites.front_default"
            :alt="pokemonData.name"
          />
        </div>
        <ul class="type">
          <h2>Type:</h2>
          <li
            v-for="(type, index) in pokemonData.types"
            :key="index"
            :class="type.type.name"
          >
            <span>{{ type.type.name }}</span>
          </li>
        </ul>
        <ul class="stats">
          <h2>Stats:</h2>
          <li v-for="(stat, index) in pokemonData.stats" :key="index">
            <span>{{ stat.stat.name }} -> {{ stat.base_stat }}</span>
          </li>
        </ul>
      </section>
    </main>
  </div>
</template>

<script>
import { pokemonApi } from "~/api/pokemonApi";

export default {
  name: "App",

  data() {
    return {
      pokemonData: {},
      pokemonID: "",
    };
  },
  methods: {
    async searchPokemon() {
      try {
        const pokemon = await fetch(`${pokemonApi}/${this.pokemonID}`).then(
          (res) => res.json()
        );
        this.pokemonData = pokemon;
        console.log(pokemon);
        return pokemon;
      } catch (error) {
        alert("Pokemon was not found!!!");
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../style/style.scss";
</style>
