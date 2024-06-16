<script setup lang="ts">
import { computed, ref } from 'vue'
import GuessInput from '@/components/GuessInput.vue'
import { DEFEAT_MESSAGE, MAX_GUESSES_COUNT, VICTORY_MESSAGE } from '@/settings'
import englishWords from '@/englishWordsWith5Letters.json'
import GuessView from './GuessView.vue'

const props = defineProps({
  wordOfTheDay: {
    type: String,
    required: true,
    validator: (wordGiven: string) => englishWords.includes(wordGiven)
  }
})
const guessesSubmitted = ref<string[]>([])

const isGameOver = computed(
  () =>
    guessesSubmitted.value.length === MAX_GUESSES_COUNT ||
    guessesSubmitted.value.includes(props.wordOfTheDay)
)

const countOfEmptyGuesses = computed(() => {
  const guessesRemaining = MAX_GUESSES_COUNT - guessesSubmitted.value.length
  return isGameOver.value ? guessesRemaining : guessesRemaining - 1
})
</script>

<template>
  <main>
    <h1 class="title">Wordle</h1>
    <ul>
      <li v-for="(guess, index) in guessesSubmitted" :key="`${index}-${guess}`">
        <GuessView :guess="guess" />
      </li>
      <li>
        <guess-input
          :disabled="isGameOver"
          @guess-submitted="(guess) => guessesSubmitted.push(guess)"
        />
      </li>
      <li v-for="i in countOfEmptyGuesses" :key="`remaining-guess-${i}`">
        <guess-view guess="" />
      </li>
    </ul>

    <p
      v-if="isGameOver"
      v-text="guessesSubmitted.includes(wordOfTheDay) ? VICTORY_MESSAGE : DEFEAT_MESSAGE"
      class="message"
    />
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.title {
  font-size: 3rem;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  margin-bottom: 0.25rem;
}

.message {
  font-size: 3rem;
  animation: end-of-game-message-animation 700ms forwards;
  white-space: nowrap;
  text-align: center;
}

@keyframes end-of-game-message-animation {
  0% {
    opacity: 0;
    transform: rotateZ(0);
  }
  100% {
    opacity: 1;
    transform: translateY(2rem);
  }
}
</style>
