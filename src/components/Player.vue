<template>
  <div v-if="currentTrack"
    class="fixed bottom-0 left-0 right-0 bg-gray-900 text-white p-4 flex flex-col dark:bg-gray-800">
    <!-- Informations sur la musique en cours -->
    <div class="flex items-center mb-4">
      <!-- Bouton Précédent -->
      <button @click="$emit('play-previous')" class="bg-gray-700 hover:bg-gray-600 text-white px-3 py-1 rounded mr-4">
        ⏮️
      </button>

      <!-- Pochette de l'album -->
      <img :src="currentTrack.coverUrl" alt="Pochette" class="w-16 h-16 rounded mr-4">

      <div>
        <h3 class="text-lg font-bold">{{ currentTrack.title }}</h3>
        <p class="text-sm">{{ currentTrack.artist }}</p>
      </div>

      <!-- Lecteur audio -->
      <audio ref="audioPlayer" :src="currentTrack.audioUrl" controls class="ml-auto"
        @ended="$emit('remove-current')"></audio>

      <!-- Bouton Suivant -->
      <button @click="$emit('play-next')" class="bg-gray-700 hover:bg-gray-600 text-white px-3 py-1 rounded ml-4">
        ⏭️
      </button>
    </div>

    <!-- File d'attente -->
    <div>
      <h2 class="text-lg font-semibold mb-2">File d'attente :</h2>
      <ul class="space-y-2">
        <li v-for="(track, index) in playlist" :key="index"
          :class="{ 'bg-green-500': track === currentTrack, 'bg-gray-800': track !== currentTrack }"
          class="flex items-center justify-between p-2 rounded dark:bg-gray-700">
          <span @click="$emit('play-music', track)" class="cursor-pointer">
            {{ track.title }} - {{ track.artist }}
          </span>
          <button @click="$emit('remove-music', index)" class="text-red-500 hover:text-red-300">
            Supprimer
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>



<script setup>
import { ref } from 'vue';

const props = defineProps({
  currentTrack: {
    type: Object,
    required: true,
  },
  playlist: {
    type: Array,
    required: true,
  },
});

const audioPlayer = ref(null);
</script>
