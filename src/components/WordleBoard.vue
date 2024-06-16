<script setup lang="ts">
import { ref } from 'vue'
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

const onInput = (event: Event) => {
  const target = event.target as HTMLInputElement
  const rawValue = target.value
  const filteredValue = rawValue
    .slice(0, WORD_SIZE)
    .toUpperCase()
    .replace(/[^A-Z]+/gi, '')
  target.value = filteredValue
  guessInProgress.value = filteredValue
}

const onSubmit = () => {
  if (!englishWords.includes(guessInProgress.value)) return
  guessSubmitted.value = guessInProgress.value
}
</script>

<template>
  <input
    v-model="guessInProgress"
    maxlength="WORD_SIZE"
    type="text"
    @input="onInput"
    @keydown.enter="onSubmit"
  />
  <p
    v-if="guessSubmitted.length > 0"
    v-text="guessSubmitted === wordOfTheDay ? VICTORY_MESSAGE : DEFEAT_MESSAGE"
  />
</template>
