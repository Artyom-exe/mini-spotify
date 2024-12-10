<template>
  <div class="grid grid-cols-2 gap-4 md:grid-cols-4 lg:grid-cols-6 dark:bg-gray-900 dark:text-white">
    <div v-for="(music, index) in musics" :key="index"
      class="p-4 border rounded-lg shadow hover:shadow-lg cursor-pointer bg-gray-800 dark:bg-gray-700">
      <img :src="music.coverUrl" alt="Pochette d'album" class="w-full rounded-md" @click="$emit('play-music', music)">
      <h2 class="mt-2 text-lg font-semibold">{{ music.title }}</h2>
      <p class="text-sm">{{ music.artist }}</p>
      <button @click="$emit('add-to-playlist', music)" class="text-blue-500 mt-2">
        Ajouter à la playlist
      </button>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  musics: {
    type: Array,
    required: true,
  },
});
</script>
