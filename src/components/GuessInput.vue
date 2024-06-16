<script setup lang="ts">
import { ref } from 'vue'
import { WORD_SIZE } from '@/settings'
import englishWords from '@/englishWordsWith5Letters.json'

const guessInProgress = ref('')
const emit = defineEmits<{
  guessSubmitted: [guess: string]
}>()

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

  emit('guessSubmitted', guessInProgress.value)
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
</template>
