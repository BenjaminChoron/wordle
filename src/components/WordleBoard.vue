<script setup lang="ts">
import { computed, ref } from 'vue'
import GuessInput from '@/components/GuessInput.vue'
import { MAX_GUESSES_COUNT } from '@/settings'
import englishWords from '@/englishWordsWith5Letters.json'
import GuessView from './GuessView.vue'
import Happy from '@/assets/happy.svg'
import Disappointed from '@/assets/disappointed.svg'

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

const resetGame = () => {
  document.location.reload()
}
</script>

<template>
  <main>
    <h1 class="title">Wordle</h1>
    <ul>
      <li v-for="(guess, index) in guessesSubmitted" :key="`${index}-${guess}`">
        <GuessView :answer="wordOfTheDay" :guess="guess" />
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

    <div v-if="isGameOver" class="layer">
      <div class="end-of-game" v-if="isGameOver && guessesSubmitted.includes(wordOfTheDay)">
        <img :src="Happy" alt="Happy face" class="image" />
        <p>Great job!</p>
        <button @click="resetGame" class="reset-game-btn">Play again</button>
      </div>

      <div class="end-of-game" v-if="isGameOver && !guessesSubmitted.includes(wordOfTheDay)">
        <img :src="Disappointed" alt="Disappointed face" class="image" />
        <p>You had to guess: {{ wordOfTheDay }}</p>
        <button @click="resetGame" class="reset-game-btn">Play again</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playwrite+NL:wght@100..400&display=swap');

main {
  width: 100%;
  height: 100dvh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: Roboto, sans-serif;
}

.title {
  font-family: 'Playwrite NL', cursive;
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

.layer {
  position: absolute;
  z-index: 2;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100dvh;
  background-color: hsla(0, 0%, 48%, 0.8);
}

.end-of-game {
  z-index: 10;
  opacity: 0;
  font-size: 2rem;
  animation: end-of-game-message-animation 700ms forwards;
  white-space: nowrap;
  text-align: center;
}

.image {
  width: 6rem;
  height: 6rem;
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
