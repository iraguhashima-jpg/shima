<template>
  <div class="game-container">

    <div class="buttons">
      <button @click="start">
        Start Game
      </button>

      <button @click="stop">
        Quit
      </button>
    </div>

    <!-- ⏱ Timer -->
    <p class="timer">
      Elapsed Time: {{ elapsedTime }}
    </p>

    <!-- 🏆 Win Message -->
    <h2 v-if="isWinning" class="win-text">
      🎉 You Have Won!!
    </h2>

    <!-- 🧩 Puzzle Grid -->
    <div class="row">

      <PuzzleTile
        v-for="(image, index) in shuffledPuzzleArray"
        :key="image"
        :image="image"
        :puzzle-id="props.puzzleId"
        @click="swap(index)"
      />

    </div>

  </div>
</template>

<script setup>
/* global defineProps */

import { ref, computed } from 'vue'
import PuzzleTile from './PuzzleTile.vue'

// ✅ reactive props
const props = defineProps({
  puzzleId: {
    type: String,
    default: 'cut-easy'
  }
})

// ✅ correct image order
const correctPuzzleArray = [
  'maker_part_001.jpg',
  'maker_part_002.jpg',
  'maker_part_003.jpg',
  'maker_part_004.jpg',
  'maker_part_005.jpg',
  'maker_part_006.jpg',
  'maker_part_007.jpg',
  'maker_part_008.jpg',
  'maker_part_009.jpg'
]

// ✅ shuffled images
const shuffledPuzzleArray = ref(
  [...correctPuzzleArray].sort(
    () => Math.random() - 0.5
  )
)

// ✅ selected indexes
const indexesToSwap = ref([])

// ✅ timer refs
const timer = ref(null)

const startDateTime = ref(new Date())

const currentDateTime = ref(new Date())

// ✅ winning logic
const isWinning = computed(() => {

  return correctPuzzleArray.every(
    (item, index) =>
      item === shuffledPuzzleArray.value[index]
  )

})

// ✅ elapsed milliseconds
const elapsedDiff = computed(() => {

  return (
    currentDateTime.value -
    startDateTime.value
  )

})

// ✅ formatted timer
const elapsedTime = computed(() => {

  const totalSeconds = Math.floor(
    elapsedDiff.value / 1000
  )

  const hours = String(
    Math.floor(totalSeconds / 3600)
  ).padStart(2, '0')

  const minutes = String(
    Math.floor((totalSeconds % 3600) / 60)
  ).padStart(2, '0')

  const seconds = String(
    totalSeconds % 60
  ).padStart(2, '0')

  return `${hours}:${minutes}:${seconds}`

})

// ✅ swap images
function swap(index) {

  // game not started
  if (!timer.value) {
    return
  }

  // maximum 2 selected images
  if (indexesToSwap.value.length < 2) {

    indexesToSwap.value.push(index)

  }

  // swap logic
  if (indexesToSwap.value.length === 2) {

    const [index1, index2] =
      indexesToSwap.value

    ;[
      shuffledPuzzleArray.value[index1],
      shuffledPuzzleArray.value[index2]
    ] = [
      shuffledPuzzleArray.value[index2],
      shuffledPuzzleArray.value[index1]
    ]

    // reset selection
    indexesToSwap.value = []

    // check winning
    if (isWinning.value) {

      saveRecord()

      stop()

    }
  }
}

// ✅ start game
function start() {

  resetTime()

  shuffledPuzzleArray.value =
    [...correctPuzzleArray]
      .sort(() => Math.random() - 0.5)

  indexesToSwap.value = []

  clearInterval(timer.value)

  timer.value = setInterval(() => {

    currentDateTime.value =
      new Date()

  }, 1000)
}

// ✅ stop game
function stop() {

  clearInterval(timer.value)

  timer.value = null
}

// ✅ reset timer
function resetTime() {

  startDateTime.value =
    new Date()

  currentDateTime.value =
    new Date()
}

// ✅ save records
function saveRecord() {

  let records =
    JSON.parse(
      localStorage.getItem('records')
    ) || []

  records.push({
    elapsedTime: elapsedTime.value,
    elapsedDiff: elapsedDiff.value
  })

  // sort fastest first
  records = records
    .sort(
      (a, b) =>
        a.elapsedDiff - b.elapsedDiff
    )
    .slice(0, 10)

  localStorage.setItem(
    'records',
    JSON.stringify(records)
  )
}
</script>

<style scoped>
.game-container {
  text-align: center;
  padding: 20px;
}

.buttons {
  margin-bottom: 15px;
}

button {
  margin: 5px;
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  background: #42b983;
  color: white;
  font-size: 16px;
}

button:hover {
  opacity: 0.9;
}

.timer {
  font-size: 20px;
  margin-bottom: 15px;
}

.win-text {
  color: green;
  margin-bottom: 15px;
}

.row {
  display: flex;
  flex-wrap: wrap;
  max-width: 70vw;
  margin: auto;
}

@media (max-width: 768px) {

  .row {
    max-width: 95vw;
  }

}
</style>