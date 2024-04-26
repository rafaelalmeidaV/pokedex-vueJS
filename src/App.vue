<script setup lang="js">
import { ref, onMounted } from 'vue'
import axios from 'axios'

const data = ref(null)

const fetchPokemon = async () => {
  try {
    const resp = await axios.get('https://pokeapi.co/api/v2/pokemon/10')
    data.value = resp.data
  } catch (error) {
    console.error(error)
  }
}

onMounted(() => {
  fetchPokemon()
})

const searchPokemon = async () => {
  const id = document.getElementById('inp').value
  console.log(id)
  if (!id) return
  try {
    const resp = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
    data.value = resp.data
  } catch (error) {
    console.error(error)
  }

}

const prevPokemon = async () => {
  const id = data.value.id - 1
  if (id < 1) return
  try {
    const resp = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
    data.value = resp.data
  } catch (error) {
    console.error(error)
  }
}

const nextPokemon = async () => {
  const id = data.value.id + 1
  if (id > 898) return
  try {
    const resp = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
    data.value = resp.data
  } catch (error) {
    console.error(error)
  }
}

</script>

<template>

  <main>
    <div v-if="data">
      <img :src="data.sprites.front_default" :alt="data.name" class="imagemPokemon" />

      <h1 class="pokemonData">
        <span class="numeroPokemon">{{ data.id }}</span> -
        <span class="nomePokemon">{{ data.name }}</span>
      </h1>

      <div class="buscaPokemon">
        <label>
          <input id="inp" class="inputSearch" placeholder="Nome ou número" required>
        </label>        
        <button class="botaovermelho" @click="searchPokemon"></button>
      </div>
      
      <div class="botao">
        <button class="botoes botaoPrev" @click = "prevPokemon">anterior << </button>
        <button class="botoes botaoNext" @click = "nextPokemon">>> proximo</button>
      </div>
      <img src="./images/pokedex.png" alt="pokedex" class="pokedex">
    </div>
  </main>

  <div v-if="data">
    <div class="redBox">
      <h2 class="infoText">Types:</h2>
      <ul>
        <li class="infoText" v-for="type in data.types" :key="type.slot">{{ type.type.name }}</li>
      </ul>
    </div>
  </div>


</template>