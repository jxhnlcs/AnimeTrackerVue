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
</script>

<template>
  <main>Hello world</main>
</template>

