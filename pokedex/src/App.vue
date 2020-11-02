<template id="teste">
  <div id="app">

    <img src="./assets/images/Pokedex.png">

    <div class="column is-half is-offset-one-quarter">

      <div class="field has-addons">

        <div class="control has-icons-left has-icons-right is-expanded">
            <input type="text" class="input is-rounded" @keyup.enter="buscar"  v-model="busca" placeholder="Buscar Pokémon pelo nome">
        </div>

        <p class="control">
            <a class="button is-info is-rounded" @click="buscar">BUSCAR</a>
        </p>

      </div>

      <div class="columns">

        <div class="column">
          <div class="content">
            <button class="button is-link is-rounded" v-if="geracao === 1" @click="trocarGeracao(1)">1ª Geração</button>
            <button class="button is-primary is-rounded" v-else @click="trocarGeracao(1)">1ª Geração</button>
          </div>
        </div>

        <div class="column">
          <div class="content">
            <button class="button is-link is-rounded" v-if="geracao === 2" @click="trocarGeracao(2)">2ª Geração</button>
            <button class="button is-primary is-rounded" v-else @click="trocarGeracao(2)">2ª Geração</button>
          </div>
        </div>

        <div class="column">
          <div class="content">
            <button class="button is-link is-rounded" v-if="geracao === 3" @click="trocarGeracao(3)">3ª Geração</button>
            <button class="button is-primary is-rounded" v-else @click="trocarGeracao(3)">3ª Geração</button>
          </div>
        </div>

        <div class="column">
          <div class="content">
            <button class="button is-link is-rounded" v-if="geracao === 4" @click="trocarGeracao(4)">4ª Geração</button>
            <button class="button is-primary is-rounded" v-else @click="trocarGeracao(4)">4ª Geração</button>
          </div>
        </div>

      </div>
      
      <div v-if="loading == true">
        <progress class="progress is-large is-info" max="100">60%</progress>
      </div>
      
      <div class="columns is-multiline" v-else>
        <div class="column is-one-third" v-for="(poke) in filteredPokemons" :key="poke.url">
          <Pokemon :pokedexNumber="getPokedexNumber(poke.url)" :name="poke.name" :url="poke.url"/>
        </div>
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
      geracao: 1,
      loading: false,
    }
  },
  created: function(){
    this.loading = true;
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(
      response => {
        this.pokemons = response.data.results;
        this.filteredPokemons = response.data.results;
      }
    );
    this.loading = false;
  },
  methods:{
    buscar: function(){
      this.filteredPokemons = this.pokemons;

      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons;
      } else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.match(this.busca.toLocaleLowerCase()))
      }
    },
    trocarGeracao: function(geracao){
      this.geracao = geracao;
      this.loading = true;
      if(this.geracao === 1){
        axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(
          response => {
            this.pokemons = response.data.results;
            this.filteredPokemons = response.data.results;
          }
        );
      } else if(this.geracao === 2){
        axios.get("https://pokeapi.co/api/v2/pokemon?limit=251&offset=151").then(
          response => {
            this.pokemons = response.data.results;
            this.filteredPokemons = response.data.results;
          }
        );
      } else if(this.geracao === 3){
        axios.get("https://pokeapi.co/api/v2/pokemon?limit=386&offset=251").then(
          response => {
            this.pokemons = response.data.results;
            this.filteredPokemons = response.data.results;
          }
        );
      } else if(this.geracao === 4){
        axios.get("https://pokeapi.co/api/v2/pokemon?limit=493&offset=386").then(
          response => {
            this.pokemons = response.data.results;
            this.filteredPokemons = response.data.results;
          }
        );
      }
      this.loading = false;
    },
    getPokedexNumber: function(url){
      var number = url.slice(0, -1);
      number = number.substring(number.lastIndexOf('/')+1);
      return `#${number}`;
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
    padding-top: 60px;
    background-color: #808B96;
  };

</style>
