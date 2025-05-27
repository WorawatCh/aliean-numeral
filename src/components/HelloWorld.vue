<template>
  <div class="container py-5">
    <h1 class="mb-4">Alien Numeral Converter</h1>

    <div class="mb-3">
      <label for="alienInput" class="form-label">Enter Alien Numeral:</label>
      <input v-model="input" type="text" id="alienInput" class="form-control" />
    </div>

    <button @click="convert" class="btn btn-primary mb-3">Convert</button>

    <div v-if="answer !== null" class="alert alert-success">
      <strong>Answer:</strong> {{ answer }}
    </div>

    <div v-if="explanation" class="alert alert-info">
      <strong>Explanation:</strong> {{ explanation }}
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const input = ref('')
const answer = ref(null)
const explanation = ref('')

const values = {
  A: 1,
  B: 5,
  Z: 10,
  L: 50,
  C: 100,
  D: 500,
  R: 1000
}

function convert() {
  const s = input.value.toUpperCase()
  let total = 0
  let explain = []
  let i = 0

  while (i < s.length) {
    const current = s[i]
    const next = s[i + 1]

    const currentVal = values[current]
    const nextVal = next ? values[next] : 0

    if (
      (current === 'A' && (next === 'B' || next === 'Z')) ||
      (current === 'Z' && (next === 'L' || next === 'C')) ||
      (current === 'C' && (next === 'D' || next === 'R'))
    ) {
      const val = nextVal - currentVal
      total += val
      explain.push(`${current}${next}=${val}`)
      i += 2
    } else {
      total += currentVal
      explain.push(`${current}=${currentVal}`)
      i += 1
    }
  }

  answer.value = total
  explanation.value = explain.join(', ')
}
</script>

<style scoped>
.container {
  max-width: 600px;
}
</style>