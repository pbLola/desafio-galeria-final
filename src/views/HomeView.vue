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
const page = ref(1)
let debounceTimer: number

watch(searchTerm, (newTerm) => {
  clearTimeout(debounceTimer)

  debounceTimer = setTimeout(() => {
    clearTimeout(debounceTimer)

    if (newTerm.length >= 2) {
      fetchImages(newTerm, true)
    }
    if (newTerm.length === 0) {
      images.value = []
    }
  }, 300)
})
const fetchImages = async (query: string, isNewSearch = false) => {
  if (isNewSearch) {
    page.value = 1
    images.value = []
  }
  isLoading.value = true
  error.value = null

  try {
    const response = await fetch(
      `https://api.pexels.com/v1/search?query=${query}&per_page=15&page=${page.value}`,
      {
        headers: {
          Authorization: apiKey,
        },
      },
    )

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
const loadMore = () => {
  page.value++
  fetchImages(searchTerm.value)
}
onMounted(() => {
  fetchImages(searchTerm.value, true)
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
      <button
        @click="fetchImages(searchTerm, true)"
        class="bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700"
      >
        Tentar Novamente
      </button>
    </div>
    <div v-else="images.length === 0" class="text-center mt-10">
      <p class="text-xl text-gray-500">Nenhum resultado encontrado para "{{ searchTerm }}"</p>
    </div>
    <GalleryGrid v-else :images="images" />
    <div ref="sentinel" class="h-10"></div>
  </main>
</template>
