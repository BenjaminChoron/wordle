<script setup lang="ts">
import { computed, ref } from 'vue'
import { DEFEAT_MESSAGE, VICTORY_MESSAGE, WORD_SIZE } from '@/settings'
import englishWords from '@/englishWordsWith5Letters.json'

defineProps({
  wordOfTheDay: {
    type: String,
    validator: (wordGiven: string) => englishWords.includes(wordGiven)
  }
})

const guessInProgress = ref('')
const guessSubmitted = ref('')

const formattedGuessInProgress = computed({
  get: () => guessInProgress.value,
  set: (rawValue: string) => {
    guessInProgress.value = rawValue.slice(0, WORD_SIZE).toUpperCase()
  }
})

const onSubmit = () => {
  if (!englishWords.includes(guessInProgress.value)) return
  guessSubmitted.value = guessInProgress.value
}
</script>

<template>
  <input
    maxlength="WORD_SIZE"
    type="text"
    v-model="formattedGuessInProgress"
    @keydown.enter="onSubmit"
  />
  <p
    v-if="guessSubmitted.length > 0"
    v-text="guessSubmitted === wordOfTheDay ? VICTORY_MESSAGE : DEFEAT_MESSAGE"
  />
</template>
