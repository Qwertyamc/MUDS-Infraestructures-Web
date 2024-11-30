<template>
    <AppBar></AppBar>
    <div class="main">
      <div v-if="data" v-for="anime in data">
        <AnimeCard :anime="anime" :favorites="favorites"></AnimeCard>
      </div>
    </div>
    
  </template>
  
  <script setup>
  import AnimeCard from '@/components/AnimeCard.vue';
  import { ref, onMounted } from 'vue';
  const data = ref(null);
  const favorites = ref(null);

  async function fetchData() {
    //let response = await fetch("http://127.0.0.1:5000/api?name=Arnau");
    data.value = await fetch("http://127.0.0.1:5000/api/getFavorites").then((response)=>response.json());
    }
  
  async function fetchFavorites() {
    if(sessionStorage.getItem('favorites')){
        favorites.value = sessionStorage.getItem('favorites')
    } else{
        let f = await fetch("http://127.0.0.1:5000/api/getFavoritesList").then((response)=>response.json())
        favorites.value = f.favorites
        sessionStorage.setItem('favorites', f.favorites);
    }
  }
  
  onMounted(() => {
    fetchFavorites();
    fetchData();
  });
  </script>
  
  <style scoped>
   .main{
      display: flex;
      flex-wrap: wrap;
      flex-direction: row;
      justify-content: center;
      gap: 3rem;
      margin-top: 3rem;
    }
  </style>