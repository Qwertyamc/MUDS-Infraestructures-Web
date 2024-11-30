<template>
<v-card class="card" v-if="anime" :id="anime.id">
    <img :src="anime.image" alt="">
    <v-btn class="btn" color="background" @click="moreInfo(anime.url)">More info</v-btn>
    <v-btn class="btn" color="primary" v-if="!favorite" @click="addFavorite(anime.id)">Add to favorites</v-btn>
    <v-btn class="btn" color='error' v-else @click="removeFavorite(anime.id)">Remove favorite</v-btn>
</v-card>
</template>

<script setup>
import { ref } from 'vue';

const {anime, favorites} = defineProps(['anime', 'favorites'])
const favorite = ref(favorites.includes(anime.id))

function moreInfo(url){
    window.open(url)
}

async function addFavorite(id){
    await fetch('http://127.0.0.1:5000/api/addFavorite', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json' // Ensure the content type is JSON
        },
        body: JSON.stringify({"id":id}) // Convert the JavaScript object to a JSON string
    })

    favorite.value = !favorite.value

    let f = await fetch("http://127.0.0.1:5000/api/getFavoritesList").then((response)=>response.json())
    sessionStorage.setItem('favorites', f.favorites);
}

async function removeFavorite(id){
    await fetch('http://127.0.0.1:5000/api/removeFavorite', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json' // Ensure the content type is JSON
        },
        body: JSON.stringify({"id":id}) // Convert the JavaScript object to a JSON string
    })

    favorite.value = !favorite.value

    let f = await fetch("http://127.0.0.1:5000/api/getFavoritesList").then((response)=>response.json())
    sessionStorage.setItem('favorites', f.favorites);
}
</script>

<style scoped>
.card{
    width: 25vw;
    height: fit-content;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
}
img{
    height: 35vh;
}
.btn{
    margin-top: 1rem;
}
</style>