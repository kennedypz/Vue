<template>
  <div id="app">
    <div class="buttons">
      <button class="button is-primary">Primary</button>
      <button class="button is-link">Link</button>
    </div>
    <h3>Cadastro: </h3>
    <small id="nomeErro" v-show="deuErro">O nome é inválido, tente novamente!</small> <br>
    <input type="text" placeholder="nome" v-model="nomeField"> <br>
    <input type="email" placeholder="email" v-model="emailField"> <br>
    <input type="number" placeholder="idade" v-model="idadeField"> <br>
    <button @click="cadastrarUsuario">Cadastrar</button>
    <hr>
    <!-- <input type="text" v-model="clienteJorge.nome"> -->
    <!-- <Cliente :nome="nomeDoJorge" email="jorge@email.com" idade="20"/> -->

    <!-- <Cliente :cliente="clienteJorge" :showIdade="true" :isPremium="false"/>
    <Cliente :cliente="clienteJorge" :showIdade="true" :isPremium="false"/>
    <Cliente :cliente="clienteJorge" :showIdade="false" :isPremium="true"/>
    <Cliente :cliente="clienteJorge" :showIdade="false" :isPremium="true"/> -->
    
    <div v-for="(c, index) in orderClientes" :key="c.id">
      <h4>{{ index + 1}}</h4>
      <Cliente :cliente="c" :showIdade="false" @meDelete="deletarUsuario($event)"/>
      <!-- <hr>
      <h4>Edição: </h4>
      <input type="text" v-model="c.nome">
      <input type="text" v-model="c.email"> -->
    </div>

  </div>
</template>

<script>
import _ from 'lodash';
import Cliente from './components/Cliente'
// import Produto from './components/Produto'

export default {
  name: 'App',
  data(){
    return {
      // nomeDoJorge: "Jorge da Silva",
      // clienteJorge:{
      //   nome: "Jorge da Silva",
      //   email: "jorge@email.com",
      //   idade: 20
      // },
      nomeField: "",
      emailield: "",
      idadeField: 0,
      clientes: [
        {
          id: 0,
          nome: "Beltrano",
          email: "cliente1@email.com",
          idade: 54
        },
        {
          id: 1,
          nome: "Zico",
          email: "cliente2@email.com",
          idade: 12
        },
        {
          id: 2,
          nome: "Adalberto",
          email: "cliente3@email.com",
          idade: 82
        },
      ],
      deuErro: false,
    }
  },
  components: {
    Cliente,
    // Produto
  },
  methods: {
    cadastrarUsuario: function() {
      if(this.nomeField == "" || this.nomeField == " " || this.nomeField < 3){
        this.deuErro = true;
      } else{
        this.deuErro = false;
        this.clientes.push({
          id: Date.now(),
          nome: this.nomeField,
          email: this.emailField,
          idade: this.idadeField
        }),
        this.nomeField = "";
        this.emailField = "";
        this.idadeField = 0;
      }
    },
    deletarUsuario: function($event){
      console.log("Recebendo evento!")
      // console.log($event);
      // console.log($event.idCliente),
      // $event.component.testar();
      var id = $event.idCliente;
      this.clientes = this.clientes.filter(cliente => cliente.id != id);
    },
  },
  computed: {
    orderClientes: function(){
      return _.orderBy(this.clientes, ['nome'], ['asc']);
    }
  }
}
</script>

<style>
  #nomeErro{
    color: red;
  }
</style>
