<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import type { PexelsPhoto } from '@/types'
import SearchBar from '@/components/SearchBar.vue'
import GalleryGrid from '@/components/GalleryGrid.vue'

const apiKey = import.meta.env.VITE_PEXELS_API_KEY
const images = ref<PexelsPhoto[]>([])
const isLoading = ref(false)
const isLoadingMore = ref(false)
const error = ref<string | null>(null)
const searchTerm = ref('nature')
const page = ref(1)
let debounceTimer: number
const sentinel = ref<HTMLElement | null>(null)

watch(searchTerm, (newTerm, oldTerm) => {
  if (newTerm === oldTerm) return

  clearTimeout(debounceTimer)

  debounceTimer = setTimeout(() => {
    const trimmedTerm = newTerm.trim()

    if (trimmedTerm.length >= 2) {
      fetchImages(trimmedTerm, true)
    }
    if (trimmedTerm.length === 0) {
      images.value = []
    }
  }, 300)
})
const handleImmediateSearch = () => {
  clearTimeout(debounceTimer)
  if (searchTerm.value.length >= 2) {
    fetchImages(searchTerm.value, true)
  }
}
const fetchImages = async (query: string, isNewSearch = false) => {
  if (isNewSearch) {
    page.value = 1
    images.value = []
    isLoading.value = true
  } else {
    isLoadingMore.value = true
  }
  error.value = null
  if (import.meta.env.VITE_USE_MOCK_MODE === 'true') {
    console.log('no modo mock')
    try {
      const response = await fetch('/mock.json')
      const data = await response.json()
      images.value.push(...data.photos)
    } catch (err) {
      error.value = 'Falha ao carregar o arquivo mock.json.'
    } finally {
      isLoading.value = false
      isLoadingMore.value = false
    }
    return
  }

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
    images.value.push(...data.photos)
  } catch (err: any) {
    error.value = err.message
    images.value = []
  } finally {
    isLoading.value = false
    isLoadingMore.value = false
  }
}
const loadMore = () => {
  page.value++
  fetchImages(searchTerm.value)
}
onMounted(() => {
  fetchImages(searchTerm.value, true)

  const observer = new IntersectionObserver((entries) => {
    const firstEntry = entries[0]

    if (firstEntry && firstEntry.isIntersecting && !isLoading.value) {
      loadMore()
    }
  })
  if (sentinel.value) {
    observer.observe(sentinel.value)
  }
})
</script>
<template>
  <main class="bg-black min-h-screen pb-12">
    <h1 class="text-8xl font-thin text-center text-white py-6 font-sans">Galeria</h1>

    <SearchBar v-model="searchTerm" @search="handleImmediateSearch" />

    <div v-if="isLoading" class="flex justify-center mt-10 px-4">
      <div class="w-ful max-w-md p-6 bg-gray-700 rounded-2xl backdrop-blur-sm text-center">
        <p class="text-xl text-gray-500">Carregando ...</p>
      </div>
    </div>

    <div v-else-if="error" class="flex justify-center mt-10 px-4">
      <div
        class="w-full max-w-lg p-6 bg-gray-900/50 border border-red-500 rounded-2xl backdrop-blur-sm text-center"
      >
        <p class="font-bold text-lg text-red-400 mb-2">Algo deu errado.</p>
        <p class="text-gray-300 mb-4">{{ error }}</p>

        <button
          @click="
            () => {
              searchTerm = 'nature'
              fetchImages('nature', true)
            }
          "
          class="bg-red-600 text-white px-5 py-3 rounded-lg hover:bg-red-700 transition-colors"
        >
          Tentar Novamente
        </button>
      </div>
    </div>

    <div v-else-if="images.length === 0" class="flex justify-center mt-10 px-4">
      <div
        class="w-full max-w-md p-6 bg-gray-900/50 border border-gray-700 rounded-2xl background-blur-sm text-center"
      >
        <p class="text-xl text-gray-300">Nenhum resultado encontrado para "{{ searchTerm }}"</p>
      </div>
    </div>

    <GalleryGrid v-else :images="images" />

    <div v-if="isLoadingMore" class="flex justify-center items-center p-6">
      <div
        class="h-8 w-8 animate-spin rounded-full border-4 border-gray-600 border-t-blur-500"
        role="status"
      >
        <span class="sr-only">Carregando</span>
      </div>
    </div>

    <div ref="sentinel" class="h-10"></div>
  </main>
</template>
