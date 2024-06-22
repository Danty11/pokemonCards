<script setup>
import Header from './components/Header.vue';
import { ref } from 'vue';
import { reactive } from 'vue';

const pokemon = reactive({})
const abilities = reactive({ability1: '',
  ability2: ''
})

const name = ref('')
const weakness = ref('')

let searched = ref(false)

 async function fetchData()
{
  //fetch the API data
  fetch(`https://pokeapi.co/api/v2/pokemon/${name.value}`)
  .then(res => res.json())
  .then(data => {Object.assign(pokemon , data) 
    fetchAbilities()
  })
  .catch(err => console.log(err))

 
  
  searched.value = true
   name.value = ""
  
}

 function fetchAbilities()
{
   fetch(`${pokemon.abilities[0].ability.url}`)
  .then(res => res.json())
  .then(data => abilities.ability1 = data.effect_entries[1].effect)
 
  fetch(`${pokemon.abilities[1].ability.url}`)
  .then(res => res.json())
  .then(data => abilities.ability2 = data.effect_entries[1].effect)

  fetch(`${pokemon.types[0].type.url}`)
  .then(res=> res.json())
  .then(data => weakness.value = data.damage_relations.double_damage_from[0].name)
}


</script>

<template>
<Header />
  <div class="search">
    <input type="text" placeholder="Enter Name" v-model="name">
    
    <button @click="fetchData">Search</button>
  </div>

  <div v-if="searched" class="result">
    <section>
        <div class="pokemon_name"> 

          <h2>{{ pokemon.name }} </h2>

         <span class="hp"> {{ pokemon?.stats[0]?.base_stat }}HP </span>

          </div>

          <div class="pokemon_img"> 
              <img :src= "pokemon.sprites.front_default" >
           
          </div>

         <div class="info">
          <p><em>transport pokemon. Length: 8'2,weight:485lbs</em></p>
        </div>

        <div class="skills">
        <p> <strong>{{ pokemon.abilities[0].ability.name }}</strong>
          {{ abilities.ability1 }}</p>
          <hr>
          <p><strong>{{ pokemon.abilities[1].ability.name }}</strong>
          {{ abilities.ability2 }}</p>
          <hr>
        </div>

        <div class="stats2">
          <p>weakness</p>
          {{ weakness }}


        </div>
        
    </section>
    
  </div>

</template>


<style scoped >
*::-webkit-scrollbar{
  color: yellow;
  
}
.search{
  margin: auto;
  width: 50%;
  margin-bottom: 30px ;
}
.result {
  margin: auto;
  width: 50%;
}


section{
  width: 300px;
 height: 450px;
  border: 10px yellow solid;
  background-color: cyan ;
}

h2{
  margin: 0;
}

.pokemon_name{
  padding: 10px 10px 0 40px ;
  display: flex ;
  justify-content: space-between;
}
.hp{
  color: red;
  font-size: 20px;
  padding-right: 30px ;
  margin-top:5px ;
}

.pokemon_img{
  
 margin: auto;
 width: 50%;
 padding: 30px;
 border: 5px yellow  solid;
}

.info{
 
  white-space: nowrap;
  margin: auto;
  width: 75%;
  background-color: rgb(238, 238, 9);
  font-size: 11px;
  font-family: sans-serif;
  
}

.skills{
  margin-left: 10px;
}
.skills strong{
  font-size: large;
}

.skills p {
  font-size: small;
  max-height: 55px;
  overflow-y: scroll ;
  
  
}



.skills hr{
  margin: 0;
}
.stats2{
  display: flex;
  flex-direction: column;
  align-items:center ;
 align-content: center;
}
.stats2 p{
  margin: 0;
  padding: 0px 10px 0px 10px;
  font-size: large;
}

</style>
