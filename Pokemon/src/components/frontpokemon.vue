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
  body {
    font-family: 'Arial', sans-serif;
    background-color: #f0f0f0;
    margin: 0;
    padding: 0;
  }
  
  .pokemon-app {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
  }
  
  .pokemon-title {
    text-align: center;
    color: #ffcb05;
    text-shadow: 2px 2px #3d7dca;
    font-size: 3em;
    margin-bottom: 30px;
  }
  
  .loading {
    text-align: center;
    font-size: 1.5em;
    color: #3d7dca;
  }
  
  .pokemon-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
    padding: 20px;
  }
  
  .pokemon-card {
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    overflow: hidden;
    transition: transform 0.3s ease;
  }
  
  .pokemon-card:hover {
    transform: translateY(-5px);
  }
  
  .pokemon-image {
    width: 100%;
    height: 150px;
    object-fit: contain;
    background-color: #f0f0f0;
  }
  
  .pokemon-name {
    text-align: center;
    color: #3d7dca;
    margin: 10px 0;
    text-transform: capitalize;
  }
  
  .pokemon-number {
    text-align: center;
    color: #666;
    margin-bottom: 10px;
  }
  
  @media (max-width: 600px) {
    .pokemon-grid {
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    }
  }
  </style>
  
  