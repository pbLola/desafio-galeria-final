<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import type { PexelsPhoto } from '@/types'
import SearchBar from '@/components/SearchBar.vue'
import GalleryGrid from '@/components/GalleryGrid.vue'

const apiKey = import.meta.env.VITE_PEXELS_API_KEY
const images = ref<PexelsPhoto[]>([])
const isLoading = ref(false)
const error = ref<string | null>(null)
const searchTerm = ref('nature')
let debounceTimer: number

watch(searchTerm, (newTerm) => {
  clearTimeout(debounceTimer)

  debounceTimer = setTimeout(() => {
    if (newTerm.length >= 2) {
      fetchImages(newTerm)
    }
    if (newTerm.length === 0) {
      images.value = []
    }
  }, 300)
})
const fetchImages = async (query: string) => {
  isLoading.value = true
  error.value = null

  try {
    const response = await fetch(`https://api.pexels.com/v1/search?query=${query}&per_page=15`, {
      headers: {
        Authorization: apiKey,
      },
    })

    if (!response.ok) {
      throw new Error('Falha ao buscar imagens')
    }

    const data = await response.json()
    images.value = data.photos
  } catch (err: any) {
    error.value = err.message
    images.value = []
  } finally {
    isLoading.value = false
  }
}
onMounted(() => {
  fetchImages(searchTerm.value)
})
</script>
<template>
  <main class="bg-gray-50 min-h-screen">
    <h1 class="text-4xl font-bold text-center text-gray-800 py-6">Galeria de Imagens</h1>
    <SearchBar v-model="searchTerm" />
    <div v-if="isLoading" class="text-center mt-10">
      <p class="text-xl text-gray-500">Carregando ...</p>
    </div>
    <div v-else-if="error" class="text-center mt-10">
      <p class="text-xl text-red-500">{{ error }}</p>
    </div>
    <GalleryGrid v-else-if="images.length > 0" :images="images" />
  </main>
</template>
