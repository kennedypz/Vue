<template>
  <div id="app">

    <img src="./assets/images/Pokedex.png">

    <div class="column is-half is-offset-one-quarter">

      <div class="field has-addons">

        <div class="control has-icons-left has-icons-right is-expanded">
            <input type="text" class="input is-rounded"  v-model="busca" placeholder="Buscar Pokémon pelo nome">
        </div>

        <p class="control">
            <a class="button is-info is-rounded" @click="buscar">BUSCAR</a>
        </p>

      </div>
      
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :pokedexNumber="index+1" :name="poke.name" :url="poke.url"/>
      </div>

    </div>

  </div>
</template>

<script>

import axios from 'axios';
import Pokemon from './components/Pokemon';

export default {
  name: 'App',
  components:{
    Pokemon
  },
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: '',
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(
      response => {
        this.pokemons = response.data.results;
        this.filteredPokemons = response.data.results;
      }
    );
  },
  methods:{
    buscar: function(){
      this.filteredPokemons = this.pokemons;

      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons;
      } else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.match(this.busca.toLocaleLowerCase()))
      }
    }
  },
  // computed: {
  //  busca em "tempo real"
  //   resultadoBusca: function(){
  //     if(this.busca == '' || this.busca == ' '){
  //       return this.pokemons;
  //     } else{
  //       return this.pokemons.filter(pokemon => pokemon.name == this.busca);
  //     }
  //   }
  // }
}
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

</style>
