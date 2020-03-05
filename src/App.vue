<template>
  <div id="app">
    <p>{{ this.dolar }}</p>
    <p>{{ this.pokemonId }}</p>
    <p>{{ this.pokemon.name }}</p>
    <img :src="this.pokemon.sprites.front_default" alt="">
    <img :src="this.pokemon.sprites.back_default" alt="">
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
      pokemonId: ''
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
