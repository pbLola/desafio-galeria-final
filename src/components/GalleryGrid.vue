<script setup lang="ts">
import { PexelsPhoto } from '@types'
import ImageCard from './ImageCard.vue'
</script>

<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 px-4">
    <ImageCard v-for="image in images" :key="image.id" :-image="image" />
  </div>
</template>
