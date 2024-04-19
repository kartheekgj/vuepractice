<template>
  <div>
    <h2>Movie Search</h2>
    <input type="text" v-model="searchTerm" placeholder="Search for movies..." />
    <button @click="searchMovies">Search</button>
    <slot name="movies" />
  </div>
</template>
<script setup>
import { ref, computed, watch } from 'vue'
const searchTerm = ref('')
const movies = ref([])
const searchMovies = async () => {
  try {
    fetch('https://www.omdbapi.com/?s=batman&apikey=713b1a6b', {
      method: 'GET',
      cors: true
    })
      .then((response) => response.json())
      .then((data) => {
        if (data.Search) {
          movies.value = data.Search
        }
      })
  } catch (error) {
    console.error('Error fetching movies:', error)
    // Handle errors appropriately, e.g., display an error message
  }
}

watch(searchTerm, async (newTerm) => {
  if (newTerm.trim() === '') {
    movies.value = [] // Clear results if the search term is empty
  } else {
    await searchMovies()
  }
})
</script>
