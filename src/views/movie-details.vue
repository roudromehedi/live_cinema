<script setup>
import { ref, reactive, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'
const props = defineProps({
  id: {
    type: String,
    required: true
  }
})
let movieDetails = reactive({})
let isLoading = ref(true)
let router = useRouter()

onMounted(async () => {
  try {
    const result = await fetch(`http://localhost:8000/movies/${parseInt(props.id)}`)
    if (result.status === 404) {
      router.push({ name: 'NotFound' })
    }
    const response = await result.json()
    movieDetails = response
    isLoading.value = false
  } catch (error) {
    console.error('Error Fetching', error)
    isLoading.value = false
  }
})

const addBooking = async (movieId, title) => {
  try {
    const data = { movieId: movieId, title: title }
    await axios.post('http://localhost:8000/bookings', data)
    console.log('Movie added successfully!')
  } catch (error) {
    console.error('Error adding movie:', error)
  }
}
</script>
<template>
  <h1 v-if="isLoading">Loading....</h1>
  <v-card v-else class="ma-2 pa-2" max-width="344">
    <v-img :src="movieDetails.poster" height="200px" cover></v-img>

    <v-card-title> {{ movieDetails.title }} </v-card-title>

    <v-card-subtitle> {{ movieDetails.year }} </v-card-subtitle>
    <v-btn @click="addBooking(movieDetails.id, movieDetails.title)">Book Now!</v-btn>
  </v-card>
</template>
