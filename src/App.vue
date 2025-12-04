<template>
  <div :class="['app', theme]">

    <aside class="sidebar">
      <h2 class="side-title">To-Do App</h2>

      <button @click="filter = 'all'" :class="{ active: filter === 'all' }">All Tasks</button>
      <button @click="filter = 'open'" :class="{ active: filter === 'open' }">Open</button>
      <button @click="filter = 'completed'" :class="{ active: filter === 'completed' }">Completed</button>

      <button class="dark-toggle" @click="toggleTheme">
        {{ theme === 'light' ? '🌙 Dark Mode' : '🌞 Light Mode' }}
      </button>
    </aside>

    <main>
      <TodoList :filter="filter" />
    </main>

  </div>
</template>

<script>
import TodoList from "./components/TodoList.vue";

export default {
  components: { TodoList },

  data() {
    return {
      filter: "all",
      theme: "light",
    };
  },

  methods: {
    toggleTheme() {
      this.theme = this.theme === "light" ? "dark" : "light";
    },
  },
};
</script>

<style>
.app {
  display: flex;
  height: 100vh;
  background: var(--bg);
  color: var(--text);
  transition: 0.3s ease;
}

:root {
  --bg: #f2f2f2;
  --text: #222;
}

.dark {
  --bg: #121212;
  --text: #fff;
}

.sidebar {
  width: 220px;
  padding: 20px;
  background: #1e1e1e;
  color: white;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.side-title {
  text-align: center;
  font-size: 22px;
  margin-bottom: 10px;
}

.sidebar button {
  padding: 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  background: #333;
  color: white;
  transition: 0.2s;
}

.sidebar button:hover {
  background: #555;
}

.active {
  background: #006cff !important;
}

.dark-toggle {
  margin-top: auto;
  background: #006cff !important;
}

main {
  flex: 1;
  padding: 40px;
}
</style>
