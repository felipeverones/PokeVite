<script setup>
import { onMounted, reactive, ref, computed } from 'vue';
import ListPokemons from '@/components/ListPokemons.vue'
import CardPokemonSelected from '@/components/CardPokemonSelected.vue'

let urlBasePNG = ref("https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/")

let pokemons = reactive(ref())

let searchPokemonField = ref("")

let pokemonSelected = reactive(ref())

let pokemonSelectedName = reactive(ref())

let loading = ref(false)


const AjeitaNome =  (pokemonName) =>{
  let nomeMaiusculo = pokemonName.charAt(0).toUpperCase() + pokemonName.slice(1).replaceAll('-',' ')

  const nomeAjeitado = nomeMaiusculo.split(" ");

  for (let i = 0; i < nomeAjeitado.length; i++) {
    nomeAjeitado[i] = nomeAjeitado[i][0].toUpperCase() + nomeAjeitado[i].substr(1);
  }

  return nomeAjeitado.join(" ")

}


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
      AjeitaNome(pokemon.name).toLowerCase().includes(searchPokemonField.value.toLowerCase())
    )
  }
  return pokemons.value;
})


const selectPokemon = async (pokemon)=>{
  loading.value = true;
  await fetch(pokemon.url)
  .then(res => res.json())
  .then(res => {
    pokemonSelected.value = res
    pokemonSelectedName = AjeitaNome(pokemonSelected.value.name)
  })
  .catch(err => alert(err))
  .finally(()=>{
    loading.value = false;
  })

}

</script>

<template>
  <main >
    <div class="container margindofooter text-body-secondary">
      
      <div class="row mt-4" >
        <div class="col-sm-12 col-md-6">

          <card-pokemon-selected
          :name="pokemonSelectedName"
          :xp="pokemonSelected?.base_experience"
          :height="pokemonSelected?.height"
          :img="pokemonSelected?.sprites.other['official-artwork'].front_default"
          :loading="loading"
          />


        </div>

        <div class="col-sm-12 col-md-6">
          <div class="mb-2 card">
                
            <label hidden for="searchPokemonField" class="form-label">Pesquisar</label>
            <input 
            type="text" 
            class="form-control" 
            id="searchPokemonField" 
            placeholder="Pesquisar"
            v-model="searchPokemonField"
            >
          </div>
          <div class="card card-list">
            <div class="card-body row">

              
              
              <ListPokemons
              v-for="(pokemon, index) in pokemonsFiltered" :key="index"
              :pokemonName="AjeitaNome(pokemon.name)"
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


/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 2px grey;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: rgb(69, 197, 220);
  border-radius: 8px;
  box-shadow: inset 0 0 5px grey;

}

.card-list{
  overflow-y: scroll;
  max-height: calc(755px - 24px - 12px - 9px);
  overflow-x: hidden ;
}

</style>