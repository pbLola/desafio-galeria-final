<script setup lang="ts">
const model = defineModel<string>()
const emit = defineEmits(['search'])
</script>
<template>
  <div class="p-8 mb-4">
    <div class="max-w-4xl mx-auto p-4 bg-gray-900/50 rounded-2xl backdrop-blur-sm">
      <div class="flex items-center">
        <input
          v-model="model"
          @keydown.enter="emit('search')"
          type="text"
          class="flex-1 bg-gray-800 border border-white text-gray-200 placeholder-gray-400 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 outline-none"
          placeholder="Buscar imagens..."
        />
        <button
          @click="emit('search')"
          type="button"
          class="p-2.5 ms-3 font-medium text-white rounded-lg border border-gray-50 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-800"
        >
          Buscar
        </button>
      </div>
    </div>
  </div>
</template>
