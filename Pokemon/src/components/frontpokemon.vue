<template>
    <div>
      <h1>Primeros 151 Pokémon</h1>
      <div v-if="loading">Cargando...</div>
      <div v-else class="pokemon-grid">
        <div v-for="pokemon in pokemons" :key="pokemon.id" class="pokemon-card">
          <img :src="pokemon.image" :alt="pokemon.name">
          <h3>{{ pokemon.name }}</h3>
          <p>Número: {{ pokemon.id }}</p>
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
  
  <style scoped>
  .pokemon-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
  }
  
  .pokemon-card {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 10px;
    text-align: center;
  }
  
  .pokemon-card img {
    max-width: 100px;
    height: auto;
  }
  </style>
  