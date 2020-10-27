<template>
  <div>
      <h1>{{pokedexNumber}} {{ name | upper }}</h1>
      <small>{{ url }}</small>
  </div>
</template>

<script>

export default {
    props:{
        pokedexNumber: Number,
        name: String,
        url: String,
    },
    filters: {
        upper: function(value){
            return value[0].toUpperCase() + value.slice(1);
        }
    }
}
</script>

<style>

</style>