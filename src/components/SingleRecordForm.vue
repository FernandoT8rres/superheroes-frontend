<template>
  <div>
    <h2>Buscar registro por ID</h2>
    <select v-model="selectedTable">
      <option disabled value="">Elige una tabla</option>
      <option v-for="table in tables" :key="table" :value="table">{{ table }}</option>
    </select>

    <input type="number" v-model="id" placeholder="Ingresa ID" />
    <button @click="fetchOne">Buscar</button>

    <div v-if="result">
      <pre>{{ result }}</pre>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const tables = ['universes', 'superheroes', 'genders', 'groups', 'powers', 'weaknesses']
const selectedTable = ref('')
const id = ref('')
const result = ref(null)

const fetchOne = async () => {
  try {
    const res = await axios.get(`http://127.0.0.1:8000/api/${selectedTable.value}/${id.value}`)
    result.value = res.data
  } catch (err) {
    console.error(err)
    result.value = { message: 'No se encontró el registro' }
  }
}
</script>
