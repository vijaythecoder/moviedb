<script setup>
import axios from 'axios'
import { ref } from 'vue'

const config = {
    headers: {
      accept: 'application/json',
      Authorization: 'Bearer TOKEN'
    }
};

const movies = ref([])
const searchInput = ref('')

const resetMovie = () => {
  axios.get('https://api.themoviedb.org/3/movie/popular', config).then(response => {
    movies.value = response.data.results
  })
}

resetMovie();

const searchMovie = () => {
  if (searchInput.value !== '') {
    axios.get('https://api.themoviedb.org/3/search/movie?query='+searchInput.value, config).then(response => {
      movies.value = response.data.results
    })
  }
  else {
    resetMovie();
  }
}
</script>

<template>
  <div class="">
    <input type="text" placeholder="Search movies" class="border-2 mx-auto w-full h-12" @input="searchMovie" v-model="searchInput">
  </div>

  <div class="container mx-auto">
    <div class="grid grid-cols-5 gap-4 mt-6">
      <div class="w-full h-96 bg-gray-200 rounded-sm" v-for="movie in movies">
        <img :src="'https://image.tmdb.org/t/p/original/' + movie.poster_path" alt="" class="w-full h-72	">
        <div class="px-4 py-2 text-gray-900 border-2 h-24 border-gray-300">
          <h2 class=" font-bold text-xl">{{ movie.title }}</h2>
          <div class="">{{ movie.release_date }}</div>
        </div>
      </div>

    </div>
  </div>
</template>
