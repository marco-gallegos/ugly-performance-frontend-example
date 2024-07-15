<script setup lang="ts">
/**
 * the goal is load a main view with the first 35 pokemons from the pokeapi
 */

import { ref, onMounted, watch } from "vue";
import PokemonInfo from './PokemonInfo.vue';

import axios from "axios";
// 35 in this page is a must
let url = "https://pokeapi.co/api/v2/pokemon/?limit=35";

const pokemonsData = ref<any[]>([]);
const pokemonSingleData = ref<any[]>([])

const loadedPokemon = ref(false);
const loading = ref(true);

async function fetchPokemon() {
    const response = await axios.get(url);

    for (const pokemon of response.data.results) {
        const newArr:any[] = [...pokemonsData.value, pokemon]
        pokemonsData.value = newArr;
    }
    loadedPokemon.value = true;
}

onMounted(async () => {
    await fetchPokemon();
})

watch(loadedPokemon , async () => {
    if (loadedPokemon.value){
        pokemonSingleData.value = [];
        for (const pokemon of pokemonsData.value) {
        const response = await axios.get(pokemon.url);
        const newPokemon = {
            ...pokemon as any,
            ...response.data,
        }
    
        const newArr = [...pokemonSingleData.value, newPokemon]
    
        pokemonSingleData.value = newArr;
        }
        console.log('finishing pokemon loading')
        loading.value = false;
    }
})


</script>

<template>
    <main v-if="!loading" >
        <div v-for="pokemon in pokemonSingleData" :key="pokemon.name">
            <PokemonInfo :pokemon="pokemon"/>
        </div>
    </main>
    <main v-else>
        Loading Data ...
    </main>
</template>