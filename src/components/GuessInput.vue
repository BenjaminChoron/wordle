<script setup lang="ts">
import { WORD_SIZE } from '@/settings'
import englishWords from '@/englishWordsWith5Letters.json'
import { ref } from 'vue'
import GuessView from '@/components/GuessView.vue'

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
  guessInProgress.value = ''
}
</script>

<template>
  <GuessView :guess="guessInProgress" />

  <input
    v-model="guessInProgress"
    :maxlength="WORD_SIZE"
    autofocus
    @blur="({ target }) => (target as HTMLInputElement).focus()"
    type="text"
    @input="onInput"
    @keydown.enter="onSubmit"
  />
</template>

<style scoped>
input {
  position: absolute;
  opacity: 0;
}
</style>
