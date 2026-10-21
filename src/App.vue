<script setup>
import { ref } from 'vue'

const frontendVersion = __APP_VERSION__
const apiUrl = import.meta.env.VITE_API_BASE_URL || 'http://localhost:8000'

const backendVersion = ref(null)
const error = ref(null)
const loading = ref(false)

async function checkHealth() {
  loading.value = true
  error.value = null
  try {
    const res = await fetch(`${apiUrl}/api/health/`)
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    backendVersion.value = (await res.json()).version
  } catch (e) {
    backendVersion.value = null
    error.value = e.message
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <main style="font-family: sans-serif; text-align: center; margin-top: 4rem">
    <h1>Hello World</h1>
    <p>Frontend version: {{ frontendVersion }}</p>
    <button :disabled="loading" @click="checkHealth">Check health</button>
    <p v-if="backendVersion">Backend version: {{ backendVersion }}</p>
    <p v-if="error" style="color: red">Error: {{ error }}</p>
  </main>
</template>
