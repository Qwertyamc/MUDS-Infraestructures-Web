<template>
    <AppBar></AppBar>
    <div class="search">
      <v-text-field
        v-model="search"
        label="Search"
        prepend-inner-icon="mdi-magnify"
        variant="outlined"
        hide-details
        single-line
        class="searchbar"
      ></v-text-field>
      <v-btn prepend-icon="mdi-magnify" @click="fetchData">
        Search
      </v-btn>
    </div>
    <div class="main">
      <div v-if="data" v-for="anime in data">
        <AnimeCard :anime="anime" :favorites="favorites"></AnimeCard>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  const data = ref(null);
  const search = ref("");
  const favorites = ref(null);
  
  async function fetchData() {
    data.value = await fetch("http://127.0.0.1:5000/api/searchAnimes?search="+search.value).then((response)=>response.json());
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
    fetchData();
    fetchFavorites();
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
  .search{
    margin-top: 3rem;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .searchbar {
    width: 20vw;
    margin-bottom: 1rem;
  }
</style>