<script setup lang="ts">
import WordleBoard from './components/WordleBoard.vue'
import englishWords from '@/englishWordsWith5Letters.json'

const wordOfTheDay = englishWords[Math.floor(Math.random() * englishWords.length)]
</script>

<template>
  <WordleBoard :wordOfTheDay="wordOfTheDay" />
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
