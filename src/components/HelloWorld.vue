<script setup>
import { ref, onMounted } from 'vue'

const people = ref([])
const person = ref(null)
const quote = ref(null)

async function loadQuotes() {
  const response = await fetch(new URL('../assets/quotes.json', import.meta.url))
  const data = await response.json()
  people.value = Array.isArray(data) ? data : []
}

function getRandomJoke() {
  if (people.value.length > 0) {
    // Pick a random person/category
    const personIdx = Math.floor(Math.random() * people.value.length)
    person.value = people.value[personIdx]
    // Pick a random quote from that person/category
    if (person.value.quotes && person.value.quotes.length > 0) {
      const quoteIdx = Math.floor(Math.random() * person.value.quotes.length)
      quote.value = person.value.quotes[quoteIdx]
    } else {
      quote.value = { text: 'No quotes available.' }
    }
  }
}

onMounted(async () => {
  await loadQuotes()
  getRandomJoke()
})
</script>

<template>
  <div class="container">
    <h1>Joke a Day</h1>
    <h2 v-if="person">{{ person.name }}</h2>
    <div v-if="quote" class="quote">
      <p>{{ quote.text }}</p>
    </div>
    <button @click="getRandomJoke">Show Another Joke</button>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 2rem auto;
  text-align: center;
}
.quote {
  margin: 2rem 0;
  font-size: 1.3rem;
  font-style: italic;
}
button {
  padding: 0.5rem 1.5rem;
  font-size: 1rem;
  cursor: pointer;
}
</style>
