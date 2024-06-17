<script setup lang="ts">
import { computed } from 'vue'
import WordleBoard from './components/WordleBoard.vue'
import englishWords from '@/englishWordsWith5Letters.json'

const wordOfTheDay = englishWords[Math.floor(Math.random() * englishWords.length)]

const isMobile = computed(() => {
  if (/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
    return true
  } else {
    return false
  }
})
</script>

<template>
  <WordleBoard v-if="!isMobile" :wordOfTheDay="wordOfTheDay" />
  <div class="not-playable">
    <p>Sorry, this game is only playable on desktop for now...</p>
    <a href="https://github.com/BenjaminChoron/wordle" class="github-link">🔗 See github project</a>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.not-playable {
  height: 100dvh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  padding: 1rem;
  font-size: 1.5rem;
}

.github-link {
  color: hsl(218, 21%, 44%);
  text-decoration: none;
}
</style>
