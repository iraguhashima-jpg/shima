<template>
  <div class="options-container">

    <h3>Select Puzzle Difficulty</h3>

    <div class="card">

      <div
        v-for="p in puzzles"
        :key="p.id"
        class="row"
      >

        <img :src="getImage(p.image)" />

        <h2>{{ p.title }}</h2>

        <button @click="selectPuzzle(p)">
          Play
        </button>

      </div>

    </div>

  </div>
</template>

<script setup>
/* global defineEmits */

import { ref } from 'vue'

const emit = defineEmits(['puzzle-changed'])

const puzzles = ref([
  {
    id: 'cut-easy',
    image: 'easy.jpg',
    title: 'Easy'
  },
  {
    id: 'cut-medium',
    image: 'medium.jpg',
    title: 'Medium'
  },
  {
    id: 'cut-hard',
    image: 'hard.jpg',
    title: 'Hard'
  }
])

function selectPuzzle(puzzle) {
  emit('puzzle-changed', puzzle.id)
}

function getImage(name) {
  return require(`../assets/${name}`)
}
</script>

<style scoped>
.card {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

.row {
  background: #f5f5f5;
  padding: 20px;
  border-radius: 10px;
}

img {
  width: 200px;
}

button {
  margin-top: 10px;
  padding: 10px;
}
</style>