<script setup lang="ts">
import type { PexelsPhoto } from '@/types'
defineProps<{
  image: PexelsPhoto
}>()
</script>
<template>
  <div class="mb-2 break-inside-avoid relative group">
    <img :src="image.src.medium" :alt="image.photographer" class="w-full rounded-2xl shadow-md" />
  </div>
</template>
