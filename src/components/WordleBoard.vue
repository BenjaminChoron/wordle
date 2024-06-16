<script setup lang="ts">
import { ref } from 'vue'
import GuessInput from '@/components/GuessInput.vue'
import { DEFEAT_MESSAGE, VICTORY_MESSAGE } from '@/settings'
import englishWords from '@/englishWordsWith5Letters.json'

defineProps({
  wordOfTheDay: {
    type: String,
    validator: (wordGiven: string) => englishWords.includes(wordGiven)
  }
})

const guessSubmitted = ref('')

const onReset = () => {
  guessSubmitted.value = ''
}
</script>

<template>
  <main>
    <h1 class="title">Wordle</h1>
    <GuessInput @guessSubmitted="(guess) => (guessSubmitted = guess)" />
    <div v-if="guessSubmitted.length > 0" class="end-of-game">
      <p
        v-text="guessSubmitted === wordOfTheDay ? VICTORY_MESSAGE : DEFEAT_MESSAGE"
        class="message"
      />
      <button @click="onReset" class="reset-btn">Play again</button>
    </div>
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
.end-of-game {
  display: flex;
  flex-direction: column;
  align-items: center;
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
.reset-btn {
  margin-top: 2rem;
  padding: 1rem 2rem;
  font-size: 1.5rem;
  background-color: hsl(0, 0%, 90%);
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: background-color 200ms;
}
.reset-btn:hover {
  background-color: hsl(0, 0%, 80%);
}
</style>
