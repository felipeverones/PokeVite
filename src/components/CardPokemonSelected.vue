<script setup>

const pokemon = defineProps(['name', 'types', 'height', 'img', 'loading'])



</script>

<template>
  
<!-- CARD -->
<div class="card CardPokemonSelected"
:class=" loading ? '' : 'animate__animated animate__flipInY'"
>
    <img
    v-if="pokemon.name"
    :src="img"
    class="card-img-top pt-2"
    :alt="name">

    <img
    src="../assets/img/Pokeball.svg"
    v-else
    class="card-img-top pt-2 pokebola"
    alt="??">

    <div class="card-body">
        <h5 class="card-title text-center">{{ pokemon.name || '???' }}</h5>
        <hr>
        <div class="row">
            <section class="col">
                <strong>Type: </strong>
                <span>{{ types || '??'}}</span>
            </section> 
            <section class="col">
                <strong>Altura: </strong>
                <span>{{ (height * 10 + ` cm`) || '??'}}</span>
            </section>
        </div>
    </div>
</div>
<!-- CARD -->

</template>



<style scoped>
.CardPokemonSelected{
    background: rgb(80,13,224);
    background: radial-gradient(circle, rgba(80,13,224,0.8267682072829132) 50%, rgba(104,242,236,0.8267682072829132) 100%);
    margin-bottom: 25px;
}

.pokebola{
    max-height: 643px;
    max-width: 634px;
}

</style>