<template>
  <div>
    <h2>Listar todos los registros</h2>
    <select v-model="selectedTable" @change="fetchAll">
      <option disabled value="">Elige una tabla</option>
      <option v-for="table in tables" :key="table" :value="table">{{ table }}</option>
    </select>

    <ul v-if="results.length">
      <li v-for="item in results" :key="item.id">
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const tables = ['universes', 'superheroes', 'genders', 'groups', 'powers', 'weaknesses']
const selectedTable = ref('')
const results = ref([])

const fetchAll = async () => {
  try {
    const res = await axios.get(`http://127.0.0.1:8000/api/${selectedTable.value}`)
    results.value = res.data
  } catch (err) {
    console.error(err)
  }
}
</script>
