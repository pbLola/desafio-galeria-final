<script setup lang="ts">
import type { PexelsPhoto } from '@/types'
defineProps<{
  image: PexelsPhoto
}>()
</script>
<template>
  <div class="bg-white rounded-lg shadow-md overflow-hidden group">
    <img :src="image.src.medium" :alt="image.photographer" class="w-full h-48 object-cover" />
    <p class="p-2 text-sm text-gray-600 truncate">{{ image.photographer }}</p>
  </div>
</template>
