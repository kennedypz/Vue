<template>
    <div :class="{'cliente': !isPremium, 'cliente-premium': isPremium}">
        <h4>Nome: {{ cliente.nome }}</h4>
        <hr>
        <p>Email: {{ cliente.email | processarEmail }}</p>
        <!-- O v-show não deleta o elemento do html, só o deixa invisível -->
        <!-- <p v-show="showIdade === true">Idade: {{ cliente.idade }}</p> -->
        <p v-if="showIdade === true">Idade: {{ cliente.idade }}</p>
        <p v-else>O usuário escondeu a idade</p>
        <button @click="mudarCor($event)">Mudar cor</button>
        <button @click="emitirEventoDelete">Deletar</button>
        <h4>ID especial: {{ idEspecial }}</h4>
    </div>
</template>

<script>
export default {
    data(){
        return {
            isPremium: false,
        }
    },
    props: {
        cliente: Object,
        showIdade: Boolean,
    },
    methods: {
        mudarCor: function($event){
            console.log($event);
            this.isPremium = !this.isPremium;
        },
        emitirEventoDelete: function(){
            console.log("Emitindo do filho");
            this.$emit(
                "meDelete", {
                    idCliente: this.cliente.id,
                    dado1: "Este é o dado 1",
                    dado2: "Este é o dado 2",
                    component: this
                }
            );
        },
        testar: function(){
            console.log("Testando");
            alert("Mensagem do alert!");
        }
    },
    filters: {
        processarEmail: function(value){
            return value.toUpperCase();
        }
    },
    computed: {
        idEspecial: function(){
            return (this.cliente.email + this.cliente.nome + this.cliente.id).toUpperCase();
        }
    }
}
</script>

<style scoped>
    .cliente{
        padding: 1%;
        margin-top: 2%;
        background-color: #ECE5E3;
        max-width: 600px;
        height: 170px;
    }

    .cliente-premium{
        padding: 1%;
        margin-top: 2%;
        background-color: black;
        color: gold;
        max-width: 600px;
        height: 170px;
    }
</style>