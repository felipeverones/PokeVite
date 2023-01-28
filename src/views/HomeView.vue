<script setup>
import { onMounted, reactive, ref, computed } from 'vue';
import ListPokemons from '@/components/ListPokemons.vue'
import CardPokemonSelected from '@/components/CardPokemonSelected.vue'

let urlBasePNG = ref("https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/")

let pokemons = reactive(ref())

let searchPokemonField = ref("")

let pokemonSelected = reactive(ref())

let loading = ref(false)

onMounted(()=>{
  fetch("https://pokeapi.co/api/v2/pokemon?limit=1271&offset=0")
  .then(res => res.json())
  .then(res => {
    pokemons.value = res.results
  })
})

const pokemonsFiltered = computed(()=>{
  if(pokemons.value && searchPokemonField.value){
    return pokemons.value.filter(pokemon=>
      pokemon.name.toLowerCase().includes(searchPokemonField.value.toLowerCase())
    )
  }
  return pokemons.value;
})


const selectPokemon = async (pokemon)=>{
  loading.value = true;
  await fetch(pokemon.url)
  .then(res => res.json())
  .then(res => pokemonSelected.value = res)
  .catch(err => alert(err))
  .finally(()=>{
    loading.value = false;
  })

  console.log(pokemonSelected.value.sprites.other['official-artwork'].front_default)
}

</script>

<template>
  <main >
    <div class="container margindofooter text-body-secondary">
      
      <div class="row mt-4" >
        <div class="col-sm-12 col-md-6">

          <card-pokemon-selected
          :name="pokemonSelected?.name"
          :xp="pokemonSelected?.base_experience"
          :height="pokemonSelected?.height"
          :img="pokemonSelected?.sprites.other['official-artwork'].front_default"
          :loading="loading"
          />


        </div>

        <div class="col-sm-12 col-md-6">
          <div class="card card-list">
            <div class="card-body row">

              <div class="mb-3">
                <label hidden for="searchPokemonField" class="form-label">Pesquisar</label>
                <input 
                type="text" 
                class="form-control" 
                id="searchPokemonField" 
                placeholder="Pesquisar"
                v-model="searchPokemonField"
                >
              </div>
              
              <ListPokemons
              v-for="(pokemon, index) in pokemonsFiltered" :key="index"
              :pokemonName="pokemon.name"
              :urlBase=" (urlBasePNG + pokemon.url.split('/')[6] + '.png')"
              @click="selectPokemon(pokemon)"
              />
            </div>

            

          </div>
        </div>
      </div>

    </div>
  </main>
</template>


<style scoped>
.margindofooter{
  margin-bottom: 75px;
}

.card-list{
  overflow-y: scroll;
  max-height: 757px;
  overflow-x: hidden ;
}

</style>