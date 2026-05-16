<template>
  <div class="column">

    <img :src="getImage()" />

  </div>
</template>

<script setup>
/* global defineProps */

const props = defineProps({
  image: String,
  puzzleId: String
})

function getImage() {
  return require(
    `../assets/${props.puzzleId}/${props.image}`
  )
}
</script>

<style scoped>
.column {
  width: 33%;
}

img {
  width: 100%;
  cursor: pointer;
}
</style>