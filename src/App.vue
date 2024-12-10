<template>
  <div>
    <!-- Barre de navigation -->
    <nav class="bg-gray-900 text-white p-4 flex justify-between items-center shadow dark:bg-gray-800">
      <div class="flex items-center space-x-4">
        <!-- Icône/logo -->
        <img src="https://via.placeholder.com/40" alt="Logo" class="w-10 h-10 rounded-full">
        <span class="text-2xl font-bold">Mini Spotify</span>
      </div>
      <div class="flex items-center space-x-4">
        <!-- Barre de recherche -->
        <input v-model="searchQuery" type="text" placeholder="Rechercher..."
          class="px-4 py-2 rounded bg-gray-700 text-white placeholder-gray-400 dark:bg-gray-600" />
        <!-- Bouton Mode Nuit -->
        <button @click="toggleDarkMode" class="bg-gray-700 hover:bg-gray-600 text-white px-4 py-2 rounded">
          {{ darkMode ? '☀️' : '🌙' }}
        </button>
      </div>
    </nav>

    <!-- Liste des musiques -->
    <MusicList :musics="filteredMusics" @play-music="playMusic" @add-to-playlist="addToPlaylist" />

    <!-- Lecteur audio -->
    <Player v-if="currentTrack" :currentTrack="currentTrack" :playlist="playlist" @play-music="playMusic"
      @remove-music="removeMusic" @remove-current="removeCurrent" @play-previous="playPrevious" @play-next="playNext"/>
    <p v-else class="text-center text-gray-500 mt-4">
      La playlist est vide. Ajoutez des musiques pour commencer !
    </p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import MusicList from './components/MusicList.vue';
import Player from './components/Player.vue';
import { musicList } from './data.js';

const currentTrack = ref(null);
const playlist = ref([]);
const searchQuery = ref("");
const darkMode = ref(false);

// Fonctions
const playMusic = (track) => {
  currentTrack.value = track;
};

const addToPlaylist = (track) => {
  if (!playlist.value.includes(track)) {
    playlist.value.push(track);
  }
};

const removeMusic = (index) => {
  playlist.value.splice(index, 1);
  if (currentTrack.value === playlist.value[index]) {
    currentTrack.value = playlist.value[0] || null;
  }
};

const removeCurrent = () => {
  const currentIndex = playlist.value.findIndex(track => track === currentTrack.value);
  playlist.value.splice(currentIndex, 1);
  currentTrack.value = playlist.value[0] || null;
};

const filteredMusics = computed(() => {
  return musicList.filter(
    (music) =>
      music.title.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      music.artist.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const toggleDarkMode = () => {
  darkMode.value = !darkMode.value;
  document.documentElement.classList.toggle("dark", darkMode.value);
};

const playPrevious = () => {
  if (playlist.value.length === 0) {
    console.warn("Playlist vide.");
    return;
  }

  const currentIndex = playlist.value.findIndex(track => track === currentTrack.value);

  const previousIndex = (currentIndex - 1 + playlist.value.length) % playlist.value.length;

  currentTrack.value = playlist.value[previousIndex];
};

const playNext = () => {
  if (playlist.value.length === 0) {
    console.warn("Playlist vide.");
    return;
  }

  const currentIndex = playlist.value.findIndex(track => track === currentTrack.value);

  const nextIndex = (currentIndex + 1) % playlist.value.length;

  currentTrack.value = playlist.value[nextIndex];
};


</script>
