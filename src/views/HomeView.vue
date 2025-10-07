<script setup lang="ts">
import { ref, onMounted } from 'vue'
import type { PexelsPhoto } from '@/types'
import SearchBar from '@/components/SearchBar.vue'
import GalleryGrid from '@/components/GalleryGrid.vue'

const images = ref<PexelsPhoto[]>([])

onMounted(async () => {
  const response = await fetch('/mock.json')
  const data = await response.json()
  images.value = data.photos
})
</script>
<template>
  <main class="bg-gray-50 min-h-screen">
    <h1 class="text-4xl font-bold text-center text-gray-800 py-6">Galeria de Imagens</h1>
    <SearchBar />
    <GalleryGrid :images="images" />
  </main>
</template>
