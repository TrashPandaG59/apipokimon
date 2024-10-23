import poke.scc

<template>
    <div class="pokemon-app">
      <h1 class="pokemon-title">Primeros 151 Pokémon</h1>
      <div v-if="loading" class="loading">Cargando Pokémon...</div>
      <div v-else class="pokemon-grid">
        <div v-for="pokemon in pokemons" :key="pokemon.id" class="pokemon-card">
          <img :src="pokemon.image" :alt="pokemon.name" class="pokemon-image">
          <h3 class="pokemon-name">{{ pokemon.name }}</h3>
          <p class="pokemon-number">Número: {{ pokemon.id }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        pokemons: [],
        loading: true
      }
    },
    mounted() {
      this.fetchPokemons()
    },
    methods: {
      async fetchPokemons() {
        try {
          const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151')
          const data = await response.json()
          
          const pokemonDetails = await Promise.all(
            data.results.map(async (pokemon) => {
              const res = await fetch(pokemon.url)
              const details = await res.json()
              return {
                id: details.id,
                name: details.name,
                image: details.sprites.front_default
              }
            })
          )
          
          this.pokemons = pokemonDetails
          this.loading = false
        } catch (error) {
          console.error('Error fetching Pokemon:', error)
          this.loading = false
        }
      }
    }
  }
  </script>
<style>
    @import "./../assets/poke.scss"; 
</style>