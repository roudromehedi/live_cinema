<script setup>
import { reactive, ref, onMounted } from 'vue'
import MovieCard from '../components/MovieCard.vue'

let movieList = reactive([])
let isLoading = ref(true)
let failedLoading = ref(false)

onMounted(() => {
  fetch('https://live-cinema.onrender.com/movies')
    .then((response) => response.json())

    .then((apiMovies) => {
      movieList = apiMovies
      isLoading.value = false
      console.log('Movie list:', movieList)
    })
    .catch((error) => {
      console.error('Error Fetching', error)
      isLoading.value = false
      failedLoading.value = true
    })
})
</script>
<template>
  <div>
    <div class="text-center mt-52" v-if="isLoading">
      <v-progress-circular
        :size="50"
        :width="5"
        indeterminate
        color="primary"
      ></v-progress-circular>
    </div>
    <h1 class="text-center mt-52" v-else-if="failedLoading">
      Failed to load movie, Please try again
    </h1>
    <v-container v-else class="">
      <v-row no-gutters>
        <v-col v-for="movie in movieList" :key="movie.id" cols="12" sm="6" md="4" lg="3" xs="12"
          ><MovieCard :movie="movie"></MovieCard>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<style lang="scss" scoped></style>
