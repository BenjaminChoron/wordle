<script setup lang="ts">
import { WORD_SIZE } from '@/settings'
import englishWords from '@/englishWordsWith5Letters.json'
import { ref } from 'vue'
import GuessView from '@/components/GuessView.vue'

withDefaults(
  defineProps<{
    disabled?: boolean
  }>(),
  {
    disabled: false
  }
)

const guessInProgress = ref('')
const hasFailedValidation = ref<boolean>(false)

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
  if (!englishWords.includes(guessInProgress.value)) {
    hasFailedValidation.value = true
    setTimeout(() => (hasFailedValidation.value = false), 500)

    return
  }

  emit('guessSubmitted', guessInProgress.value)
  guessInProgress.value = ''
}
</script>

<template>
  <GuessView v-if="!disabled" :class="{ shake: hasFailedValidation }" :guess="guessInProgress" />

  <input
    v-model="guessInProgress"
    :maxlength="WORD_SIZE"
    :disabled="disabled"
    aria-label="Make your guess for the word of the day!"
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

.shake {
  animation: shake;
  animation-duration: 100ms;
  animation-iteration-count: 2;
}

@keyframes shake {
  0% {
    transform: translateX(-2%);
  }
  25% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(2%);
  }
  75% {
    transform: translateX(0);
  }
}
</style>
