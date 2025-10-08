<script setup lang="ts">
import type { PexelsPhoto } from '@/types'
import ImageCard from './ImageCard.vue'

defineProps<{
  images: PexelsPhoto[]
}>()
</script>

<template>
  <div class="columns-1 sm:columns-2 md:columns-3 lg:columns-4 gap-1.5 px-4">
    <ImageCard v-for="image in images" :key="image.id" :image="image" />
  </div>
</template>
