<template>
  <div id="app">
    <div class="poke-card">
      <p>Dolár Compra: $ {{ this.bidDolar }}</p>
      <p>{{ this.bidPokemonId }}</p>
      <p>{{ this.bidPokemon.name }}</p>
      <img :src="this.bidPokemonFront" alt="">
      <img :src="this.bidPokemonBack" alt="">
    </div>

    <div class="poke-card">
      <p>Dolár Venda: $ {{ this.askDolar }}</p>
      <p>{{ this.askPokemonId }}</p>
      <p>{{ this.askPokemon.name }}</p>
      <img :src="this.askPokemonFront" alt="">
      <img :src="this.askPokemonBack" alt="">
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      bidDolar: '',
      bidPokemon: {},
      bidPokemonId: '',
      bidPokemonFront: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/132.png',
      bidPokemonBack: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/132.png',
      askDolar: '',
      askPokemon: {},
      askPokemonId: '',
      askPokemonFront: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/132.png',
      askPokemonBack: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/132.png'
    }
  },
  created: function () {
    axios.get('https://economia.awesomeapi.com.br/jsonp/USD-BRL')
      .then((result) => {
        console.log(result)
        this.bidDolar = Number(result.data[0].bid).toFixed(2)
        this.askDolar = Number(result.data[0].ask).toFixed(2)
        this.bidPokemonId = this.bidDolar.replace('.', '')
        this.askPokemonId = this.askDolar.replace('.', '')

        axios.get(`https://pokeapi.co/api/v2/pokemon/${this.bidPokemonId}`)
          .then((result) => {
            this.bidPokemon = result.data
            this.bidPokemonFront = this.bidPokemon.sprites.front_default
            this.bidPokemonBack = this.bidPokemon.sprites.back_default
          })

        axios.get(`https://pokeapi.co/api/v2/pokemon/${this.askPokemonId}`)
          .then((result) => {
            this.askPokemon = result.data
            this.askPokemonFront = this.askPokemon.sprites.front_default
            this.askPokemonBack = this.askPokemon.sprites.back_default
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
