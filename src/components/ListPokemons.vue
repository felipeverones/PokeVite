<script setup>

const pokemon = defineProps(['pokemonName', 'urlBase'])

</script>


<template>
  <div class="col-4">
    <div class="card p-2 mb-3 cardListPokemon">
        <p class="text-center">{{ pokemonName }}</p>
        <img :src="urlBase || pokemonName" class="card-img-top"
         alt="image not found">
    </div>
  </div>
</template>



<style scoped>
    .cardListPokemon{
        background: rgb(80,13,224);
        background: radial-gradient(circle, rgba(80,13,224,0.8267682072829132) 50%, rgba(104,242,236,0.8267682072829132) 100%);
        cursor: pointer;
    }

    .cardListPokemon:hover{
        background: rgb(80,13,224);
        background: radial-gradient(circle, rgba(80,13,224,1) 50%, rgba(104,242,236,1) 100%);
        cursor: pointer;
    }

    .cardListPokemon img:hover{
        padding: .1rem;
    }

</style>