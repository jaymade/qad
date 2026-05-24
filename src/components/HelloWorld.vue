<script setup>
import { ref, onMounted } from 'vue'

const people = ref([])
const person = ref(null)
const quote = ref(null)
const imageUrl = ref('')
const selectedPersonName = ref('All')
const showSelectors = ref(false)

const techStack = [
  { name: 'Vue 3',      url: 'https://vuejs.org',                                              logo: 'https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/vuedotjs.svg',    color: '#42b883' },
  { name: 'Vite',       url: 'https://vitejs.dev',                                             logo: 'https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/vite.svg',        color: '#646cff' },
  { name: 'JavaScript', url: 'https://developer.mozilla.org/en-US/docs/Web/JavaScript',        logo: 'https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/javascript.svg',  color: '#f7df1e' },
  { name: 'CSS3',       url: 'https://developer.mozilla.org/en-US/docs/Web/CSS',               logo: 'https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/css3.svg',        color: '#1572b6' },
  { name: 'HTML5',      url: 'https://developer.mozilla.org/en-US/docs/Web/HTML',              logo: 'https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/html5.svg',       color: '#e34f26' },
  { name: 'JSON',       url: 'https://www.json.org',                                           logo: 'https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/json.svg',        color: '#aaa' },
]

async function loadQuotes() {
  const response = await fetch(new URL('../assets/quotes.json', import.meta.url))
  const data = await response.json()
  people.value = Array.isArray(data) ? data : []
}

function getRandomJoke() {
  let filteredPeople = people.value
  if (selectedPersonName.value !== 'All') {
    filteredPeople = people.value.filter(p => p.name === selectedPersonName.value)
  }
  if (filteredPeople.length > 0) {
    const personIdx = Math.floor(Math.random() * filteredPeople.length)
    person.value = filteredPeople[personIdx]
    if (person.value.quotes && person.value.quotes.length > 0) {
      const quoteIdx = Math.floor(Math.random() * person.value.quotes.length)
      quote.value = person.value.quotes[quoteIdx]
    } else {
      quote.value = { text: 'No quotes available.' }
    }
    if (person.value.name) {
      const imgName = person.value.name.replace(/\s+/g, '') + '.png'
      imageUrl.value = new URL(`../assets/img/${imgName}`, import.meta.url).href
    } else {
      imageUrl.value = ''
    }
  }
}

function selectPerson(name) {
  selectedPersonName.value = name
  getRandomJoke()
}

onMounted(async () => {
  await loadQuotes()
  getRandomJoke()
})
</script>

<template>
  <div class="container">
    <h1>Quote a Day</h1>
    <div style="margin-bottom: 1rem;">
      <button
        @click="showSelectors = !showSelectors"
        style="margin-bottom: 0.5rem; background: none; border: none; color: #fff; font-size: 1.2rem; cursor: pointer;"
        aria-label="Toggle person selectors"
      >
        <span v-if="showSelectors">−</span>
        <span v-else>+</span>
        <span style="margin-left: 0.5rem;">Person Selectors</span>
      </button>
      <div v-if="showSelectors" class="person-toggles">
        <button
          :class="['toggle-btn', { active: selectedPersonName === 'All' }]"
          @click="selectPerson('All')"
        >
          All
        </button>
        <button
          v-for="p in people"
          :key="p.name"
          :class="['toggle-btn', { active: selectedPersonName === p.name }]"
          @click="selectPerson(p.name)"
        >
          {{ p.name }}
        </button>
      </div>
    </div>
    <img v-if="imageUrl" :src="imageUrl" :alt="person?.name" class="person-img" />
    <h2 v-if="person">{{ person.name }}</h2>
    <div v-if="quote" class="quote">
      <p>{{ quote.text }}</p>
    </div>
    <button @click="getRandomJoke">Show Another Joke</button>

    <footer class="tech-footer">
      <div class="tech-badges">
        <span class="footer-label">Built with</span>
        <a
          v-for="tech in techStack"
          :key="tech.name"
          :href="tech.url"
          target="_blank"
          rel="noopener noreferrer"
          class="badge"
          :style="{ '--logo-color': tech.color }"
        >
          <img :src="tech.logo" :alt="tech.name + ' logo'" class="badge-logo" />
          <span class="badge-name">{{ tech.name }}</span>
        </a>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 2rem auto;
  text-align: center;
  background: #000;
  color: #fff;
  border-radius: 12px;
  padding: 2rem;
}
.person-toggles {
  margin-bottom: 1rem;
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  justify-content: center;
}
.toggle-btn {
  background: #222;
  color: #fff;
  border: 1px solid #444;
  border-radius: 20px;
  padding: 0.4rem 1.2rem;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}
.toggle-btn.active,
.toggle-btn:hover {
  background: #fff;
  color: #000;
  border-color: #fff;
}
.person-img {
  width: 100%;
  max-width: 240px;
  height: auto;
  object-fit: contain;
  margin: 1rem auto;
  display: block;
}
h2 {
  color: orange;
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
  border-radius: 20px;
  border: none;
  background: #444;
  color: #fff;
  margin: 0.5rem;
  transition: background 0.2s, color 0.2s;
}
button:hover {
  background: orange;
  color: #000;
}

.tech-footer {
  margin-top: 2rem;
  padding-top: 1rem;
  border-top: 1px solid #222;
}
.tech-badges {
  display: flex;
  flex-wrap: nowrap;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}
.footer-label {
  font-size: 0.72rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #444;
  white-space: nowrap;
}
.badge {
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  color: #555;
  font-size: 0.78rem;
  text-decoration: none;
  white-space: nowrap;
  transition: color 0.2s;
}
.badge:hover {
  color: var(--logo-color);
}
.badge-logo {
  width: 13px;
  height: 13px;
  filter: invert(1) brightness(0.45);
  transition: filter 0.2s;
}
.badge:hover .badge-logo {
  filter: none;
}
.badge-name {
  font-weight: 500;
}
</style>
