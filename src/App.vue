<script setup>
import { ref } from 'vue'
import axios from 'axios'

const selectedTable = ref('')
const recordId = ref('')
const allData = ref([])
const recordData = ref(null)

const tables = ['superheroes', 'superpowers', 'weaknesses', 'universes', 'villains', 'weapons']

const fetchAll = async () => {
  if (!selectedTable.value) return
  try {
    const res = await axios.get(`http://127.0.0.1:8000/api/${selectedTable.value}`)
    allData.value = res.data
    recordData.value = null
  } catch (error) {
    console.error(error)
  }
}

const fetchById = async () => {
  if (!selectedTable.value || !recordId.value) return
  try {
    const res = await axios.get(`http://127.0.0.1:8000/api/${selectedTable.value}/${recordId.value}`)
    recordData.value = res.data
    allData.value = []
  } catch (error) {
    console.error(error)
  }
}
</script>

<template>
  <div class="container">
    <h1>Superheroes API Viewer</h1>

    <div class="form">
      <h2>List All Records</h2>
      <select v-model="selectedTable">
        <option disabled value="">Select a table</option>
        <option v-for="table in tables" :key="table">{{ table }}</option>
      </select>
      <button @click="fetchAll">Fetch All</button>
    </div>

    <div class="form">
      <h2>Get Record by ID</h2>
      <input v-model="recordId" type="number" placeholder="Enter ID" />
      <button @click="fetchById">Fetch by ID</button>
    </div>

    <div class="results">
      <div v-if="allData.length">
        <h3>All Records:</h3>
        <pre>{{ allData }}</pre>
      </div>

      <div v-if="recordData">
        <h3>Record:</h3>
        <pre>{{ recordData }}</pre>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 1rem;
}

.form {
  margin-bottom: 2rem;
}

select, input {
  margin-right: 1rem;
}

button {
  padding: 0.5rem 1rem;
}

.results pre {
  background-color: #f4f4f4;
  padding: 1rem;
  border-radius: 8px;
  overflow-x: auto;
}
</style>
