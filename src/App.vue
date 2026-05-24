<script setup>
import HelloWorld from "./components/HelloWorld.vue";

const techStack = [
  {
    name: "Vue 3",
    url: "https://vuejs.org",
    logo: "https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/vuedotjs.svg",
    color: "#42b883",
  },
  {
    name: "Vite",
    url: "https://vitejs.dev",
    logo: "https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/vite.svg",
    color: "#646cff",
  },
  {
    name: "JavaScript",
    url: "https://developer.mozilla.org/en-US/docs/Web/JavaScript",
    logo: "https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/javascript.svg",
    color: "#f7df1e",
  },
  {
    name: "CSS3",
    url: "https://developer.mozilla.org/en-US/docs/Web/CSS",
    logo: "https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/css3.svg",
    color: "#1572b6",
  },
  {
    name: "HTML5",
    url: "https://developer.mozilla.org/en-US/docs/Web/HTML",
    logo: "https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/html5.svg",
    color: "#e34f26",
  },
  {
    name: "JSON",
    url: "https://www.json.org",
    logo: "https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/json.svg",
    color: "#aaa",
  },
];
</script>

<template>
  <div class="app-wrapper">
    <main>
      <HelloWorld msg="Vite + Vue" />
    </main>

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
.app-wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

main {
  flex: 1;
}

main :deep(.container) {
  margin-bottom: 0;
  border-radius: 12px 12px 0 0;
}

.tech-footer {
  width: 600px;
  margin: 0 auto;
  padding: 0.75rem 2rem;
  background: #000;
  border-top: 1px solid #222;
  text-align: center;
  border-radius: 0 0 12px 12px;
  box-sizing: border-box;
}

@media (max-width: 700px) {
  .tech-footer {
    width: calc(100% - 4rem);
  }
}

@media (max-width: 480px) {
  .tech-footer {
    width: 100%;
    border-radius: 0;
    padding: 0.75rem 1rem;
  }
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
  margin-right: 0.25rem;
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
