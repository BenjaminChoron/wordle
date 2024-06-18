<script setup lang="ts">
import { QWERTY_KEYS } from '@/settings'

const qwertyKeysArray = QWERTY_KEYS.split('')
</script>

<template>
  <div class="virtual-keyboard">
    <button
      v-for="key in qwertyKeysArray"
      :key="key"
      :aria-label="`Type ${key}`"
      @click="$emit('type', key)"
      class="key"
      :disabled="key === ' '"
    >
      {{ key }}
    </button>
  </div>
</template>

<style lang="scss" scoped>
.virtual-keyboard {
  position: fixed;
  bottom: 1rem;
  margin: auto;
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  gap: 0.2rem;
  margin-top: 1rem;
}

.key {
  width: 1.92rem;
  padding: 0.5rem;
  font-size: 1rem;
  text-transform: uppercase;
  border: 1px solid #000;
  border-radius: 0.5rem;
  cursor: pointer;
  background-color: #fff;
  touch-action: manipulation; // prevent zoom when double tapping
  transition: background-color 0.2s;

  &:disabled {
    border: none;
    cursor: default;

    &:hover {
      background-color: #fff;
    }
  }

  &:hover {
    background-color: #f0f0f0;
  }
}
</style>
