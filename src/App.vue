<script setup>

import { ref, computed, onMounted }from 'vue'

const query = ref('')
const my_anime = ref('')
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

  localStorage.setTime('my anime', JSON.stringify(my_anime.value))
}


</script>

<template>
  <main>Hello world</main>
</template>

