<script setup>
import { ref, onMounted, watch } from 'vue'

const myArray = ref([])
const movieTitle = ref('')
const movieRating = ref(null)
const name = ref('')

const addMovie = () => {
  if (movieTitle.value.trim() === '' || movieRating.value < 1 || movieRating.value > 5) {
    alert('Please enter a valid movie title and rating between 1 and 5.')
    return
  }

  myArray.value.push({
    title: movieTitle.value,
    rating: movieRating.value,
    done:false,
  })

  movieTitle.value = ''
  movieRating.value = null
}

const deleteMovie = (x) => {
  myArray.value = myArray.value.filter(Element => Element !== x)
}

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  myArray.value = JSON.parse(localStorage.getItem('myArray')) || []
})

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(myArray, (newVal) => {
  localStorage.setItem('myArray', JSON.stringify(newVal))
}, { deep: true })

</script>

<template>

  <main class="app">

    <h1 class="main-title">Movie Ranking</h1>
    <h2 class="sub-title">By Victor Ruiz</h2>

    <section class="create-movie">
      <label class="input-label">Movie Title:
        <input type="text" placeholder="Enter movie title" v-model="movieTitle" />
      </label>

      <div class="rating-container">
        <label class="rating-label">Rating:</label>
        <div class="rating-options">
          <button v-for="n in 5" :key="n"
                  :class="{ 'selected': movieRating === n }"
                  @click="movieRating = n">
            {{ n }} <span class="star">★</span>
          </button>
        </div>
      </div>

      <button class="add-button" @click="addMovie">Add Movie</button>
    </section>

    <h2 class="tracked-title">Movies</h2>

    <section class="movie-list">
      <div v-for="x in myArray" :class=" `movie-item ${x.done ? 'done' : 'not-don'}`" :key="x">
        <span>{{ x.title }} - {{ x.rating }} Star<span v-if="x.rating > 1">s</span></span>
        <button class="delete-button" @click="deleteMovie(x)">Delete</button>
      </div>
    </section>
  </main>
</template>