<template>
<div>
    <input type="text" v-model="searchTerm" placeholder="Search Movies"/>

    <div v-if="moviesData.length > 0">
        <div class="moviesContainer" v-for="movie in moviesData" :key="movie.id">
            {{ movie.Title }}
        </div>
    </div>
    <div v-else>No Movies Found Start searching</div>
    
</div>
</template>
<script setup>
import {ref, watch} from 'vue';
const moviesData = ref([])
const searchTerm = ref('');

const fnGetMovies = (newsearch) =>{
    fetch("https://www.omdbapi.com/?s="+newsearch+"&apikey=713b1a6b", {
      method: "GET",
      cors: true,
    })
      .then((response) => response.json())
      .then((data) => {
        if(data.Search){
          moviesData.value = data.Search
        }
      });
}

watch(searchTerm, async (newsearch)=>{
    if(newsearch.trim().length === 0){
        moviesData.value = [];
    }else{
        fnGetMovies(newsearch);
    }
})

</script>