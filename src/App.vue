<script setup>

import { ref, computed, onMounted }from 'vue'

const query = ref('')
const my_anime = ref([])
const search_results = ref([])

const my_anime_asc = computed(() =>{
  return my_anime.value.sort((a, b) =>{
    return a.title.localCompare(b.title)
  })
})

const searchAnime = () => {
  const url = `https://api.jikan.moe/v4/anime?q=${query.value}`
  fetch(url)
  .then(res => res.json())
  .then(res => {
    search_results.value = res.data
  })
}

const handleInput = e => {
  if (!e.target.value){
    search_results.value = []
  }
}

const addAnime = anime => {
  search_results.value = []
  query.value = ''

  my_anime.value.push({
    id: anime.mal_id,
    title: anime.title,
    image: anime.images.jpg.image_url,
    total_episodes: anime.episodes,
    watched_epidodes: 0
  })

  localStorage.setItem('my anime', JSON.stringify(my_anime.value))
}

const increaseWatch = anime => {
  anime.watched_epidodes++
  localStorage.setItem('my anime', JSON.stringify(my_anime.value))
}

const decreaseWatch = anime => {
  anime.watched_epidodes--
  localStorage.setItem('my anime', JSON.stringify(my_anime.value))
}
onMounted(() => {
  my_anime.value = JSON.prse(localStorage.getItem('my_anime')) || []
})

</script>

<template>
  <main>
    <h1>My Anime Tracker</h1>

    <form @submit.prevent="searchAnime">
      <input 
      placeholder="Pesquise por um anime..." 
      v-model="query" 
      @input="handleInput" 
      type="text"
      />
      <button type="submit">Search</button>
    </form>
    <div class="results" v-if="search_results.length > 0">
      <div class="result" v-for="anime in search_results">
        <img :src="anime.images.jpg.image_url" alt="Anime image">
        <div class="details">
          <h3>{{ anime.title }}</h3>
          <p :title="anime.synopsis" v-if="anime.synopsis"> {{ anime.synopsis.slice(0, 180) }}... </p>
          <span class="flex-1"></span>
          <button class="button" @click="addAnime(anime)">Add to my anime</button>
        </div>
      </div>
    </div>

    <div class="myanime" v-if="my_anime.length > 0">
      <h2>My anime</h2>

      <div v-for="anime in my_anime_asc" class="anime">
        <img :src="anime.image" />
        <h3>{{ anime.title }}</h3>
        <div class="flex-1"></div>
        <span class="episodes">
          {{ anime.watched_epidodes }} / {{ anime.total_episodes }}
        </span>
        <button 
					v-if="anime.total_episodes !== anime.watched_episodes" 
					@click="increaseWatch(anime)" class="button">+</button>
				<button 
					v-if="anime.watched_episodes > 0"
					@click="decreaseWatch(anime)" class="button">-</button>
      </div>
    </div>
  </main>
</template>

