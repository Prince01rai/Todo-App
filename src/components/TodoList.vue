<template>
  <div class="todo-wrapper">
    <div class="todo-card">
      <h1 class="title">To Do List</h1>

      <div class="add-box">
        <input v-model="newTask" placeholder="Add task..." @keyup.enter="addTask" />
        <button @click="addTask">Add</button>
      </div>

      <h2 class="task-title">Task List</h2>

      <div class="task-container">
        <div 
          v-for="task in filtered" 
          :key="task.id" 
          class="task-item"
          :class="{ done: task.completed }"
        >
          <div class="task-left">
            <input type="checkbox" v-model="task.completed" @change="update(task)" />
            
            <div class="task-info">
              <span class="task-name">{{ task.title }}</span>
              <p class="date">Created: {{ formatDate(task.createdAt) }}</p>
            </div>
          </div>

          <button class="delete-btn" @click="remove(task.id)">Delete</button>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["filter"],

  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },

  computed: {
    filtered() {
      if (this.filter === "open") return this.tasks.filter(t => !t.completed);
      if (this.filter === "completed") return this.tasks.filter(t => t.completed);
      return this.tasks;
    }
  },

  methods: {
    formatDate(dateStr) {
      const d = new Date(dateStr);
      return d.toLocaleString();
    },

    async load() {
      const res = await axios.get("http://localhost:3000/tasks");
      this.tasks = res.data;
    },

    async addTask() {
      if (!this.newTask.trim()) return;

      await axios.post("http://localhost:3000/tasks", {
        title: this.newTask,
        completed: false,
        createdAt: new Date(),
      });

      this.newTask = "";
      this.load();
    },

    async update(task) {
      await axios.put(`http://localhost:3000/tasks/${task.id}`, task);
      this.load();
    },

    async remove(id) {
      await axios.delete(`http://localhost:3000/tasks/${id}`);
      this.load();
    }
  },

  mounted() {
    this.load();
  }
};
</script>

<style>
.todo-wrapper {
  display: flex;
  justify-content: center;
  padding: 40px 0;
  background: #f5f5f5;
  min-height: 100vh;
}

.todo-card {
  background: #ffffff;
  padding: 30px;
  border-radius: 16px;
  width: 500px;
  box-shadow: 0 0 20px #0002;
}

.title {
  text-align: center;
  font-size: 26px;
  margin-bottom: 20px;
}

.add-box {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}
.add-box input {
  flex: 1;
  padding: 10px;
  border-radius: 6px;
  border: 1px solid #ddd;
}
.add-box button {
  background: #006cff;
  color: white;
  border: none;
  padding: 10px 16px;
  border-radius: 6px;
  font-weight: 600;
}

.task-title {
  text-align: center;
  font-size: 18px;
  margin-bottom: 10px;
}

.task-container {
  max-height: 420px;   
  overflow-y: auto;    
  padding-right: 10px;
}

.task-item {
  background: #fbfbfb;
  padding: 14px;
  border-radius: 10px;
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #eee;
}

.task-left {
  display: flex;
  gap: 10px;
}

.task-name {
  font-size: 16px;
  font-weight: 500;
}

.date {
  font-size: 12px;
  color: gray;
}

.delete-btn {
  border: none;
  background: none;
  color: #ff3b3b;
  font-weight: 600;
  cursor: pointer;
}

.done .task-name {
  text-decoration: line-through;
  opacity: 0.6;
}
</style>
