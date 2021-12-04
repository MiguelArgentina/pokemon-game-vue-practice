<template>
  <h1 v-if="!pokemon">Buscando un Pokemon...</h1>
  <div v-else>
    <h1>¿quién es este Pokemon?</h1>

    <PokemonPicture
    :pokemonId="pokemon.id"
    :showPokemon="showPokemon"/>

    <PokemonOptions
    :pokemons="pokemonAry"
    @selection="checkAnswer($event)"/>
  </div>
  <div>
    <h2>{{message}}</h2>
    <button
    v-show="showPokemon"
    @click="resetGame">Nuevo juego</button>
  </div>
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture'
import PokemonOptions from '@/components/PokemonOptions'
import getPokemonOptions from '@/helpers/getPokemonOptions.js'

export default {
  name: 'PokemonPage',
  components: {
    PokemonPicture,
    PokemonOptions
  },
  data() {
    return {
      pokemonAry: [],
      pokemon: null,
      showPokemon: false,
      message: ''
    }
  },
  methods: {
    async mixPokemonsArray() {
      this.pokemonAry = await getPokemonOptions()
      const rndInt = Math.floor((Math.random()*40)/10)
      this.pokemon = this.pokemonAry[rndInt]
    },
    checkAnswer(pokemonId) {
      if(pokemonId == this.pokemon.id) {
        this.showPokemon = true
        this.message = `Felicidades, adivinaste, es ${this.pokemon.name}`
      }
      else {
        this.message = "Ups...inténtalo nuevamente!"
        setTimeout(() => this.message = null, 500)
      }
    },
    resetGame() {
      this.message = null
      this.showPokemon = false
      this.pokemon = null
      this.mixPokemonsArray()
    }
  },
  mounted() {
    this.mixPokemonsArray()
  }
}
</script>
<style scoped>
button {
    background-color: rgb(188, 224, 163);
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.2);
    cursor: pointer;
    margin-bottom: 10px;
    width: 100px;
    line-height: 1.5rem;
    color: rgb(53, 85, 43);
}

button:hover {
    background-color: rgba(0, 0, 0, 0.05);
}
</style>