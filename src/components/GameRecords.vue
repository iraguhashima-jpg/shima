<template>
  <div>

    <h3>Records</h3>

    <button
      class="update-button"
      @click="getRecords"
    >
      Update
    </button>

    <div
      v-for="(r, index) in records"
      :key="index"
    >
      {{ index + 1 }} -
      {{ r.elapsedTime }}
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const records = ref([])

function getRecords() {

  const data =
    JSON.parse(
      localStorage.getItem('records')
    ) || []

  records.value = data
}

onMounted(() => {
  getRecords()
})
</script>

<style scoped>
.update-button {
  margin-bottom: 20px;
  border-radius: 25px;
  background: darkslategrey;
  border: none;
  color: white;
  padding: 10px 20px;
}
</style>