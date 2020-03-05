<template>
  <div id="app">
    <p>Dolár: $ {{ this.dolar }}</p>
    <p>{{ this.pokemonId }}</p>
    <p>{{ this.pokemon.name }}</p>
    <img :src="this.pokemonFront" alt="">
    <img :src="this.pokemonBack" alt="">
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      dolar: '',
      pokemon: {},
      pokemonId: '',
      pokemonFront: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/132.png',
      pokemonBack: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/132.png'
    }
  },
  created: function () {
    axios.get('http://economia.awesomeapi.com.br/jsonp/USD-BRL')
      .then((result) => {
        this.dolar = Number(result.data[0].high).toFixed(2)
        this.pokemonId = this.dolar.replace('.', '')

        axios.get(`https://pokeapi.co/api/v2/pokemon/${this.pokemonId}`)
          .then((result) => {
            console.log(result)
            this.pokemon = result.data
            this.pokemonFront = this.pokemon.sprites.front_default
            this.pokemonBack = this.pokemon.sprites.back_default
          })
      })
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
