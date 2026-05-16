<template>
  <div class="app">

    <h1>🧩 Swap The Images To Win</h1>

    <PuzzleSelector
      @puzzle-changed="changePuzzle"
    />

    <PuzzleGame
      :puzzle-id="selectedPuzzle"
    />

    <GameRecords />

  </div>
</template>

<script setup>
import { ref } from 'vue'

import PuzzleSelector from './components/PuzzleSelector.vue'
import PuzzleGame from './components/PuzzleGame.vue'
import GameRecords from './components/GameRecords.vue'

const selectedPuzzle = ref('cut-easy')

function changePuzzle(id) {
  selectedPuzzle.value = id
}
</script>

<style scoped>
.app {
  text-align: center;
  padding: 20px;
}
</style>