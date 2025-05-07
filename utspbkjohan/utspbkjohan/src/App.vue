<template>
  <div class="app-container">
    <div class="todo-card">
      <h1>🌟 Daftar Kegiatan 🌟</h1>

      <form @submit.prevent="addTodo" class="todo-form">
        <input v-model="newTodo" placeholder="➕ Tambah kegiatan..." />
        <button type="submit">
          <span>Tambah</span>
        </button>
      </form>

      <div class="filter">
        <label>
          <input type="checkbox" v-model="showUnfinishedOnly" />
          Tampilkan hanya yang belum selesai
        </label>
      </div>

      <transition-group name="fade" tag="ul" class="todo-list">
        <li
          v-for="(todo, index) in filteredTodos"
          :key="index"
          class="todo-item"
        >
          <input type="checkbox" v-model="todo.completed" />
          <span :class="{ done: todo.completed }">{{ todo.text }}</span>
          <button class="delete-btn" @click="removeTodo(index)">🗑 Hapus</button>
        </li>
      </transition-group>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const todos = ref([])
const showUnfinishedOnly = ref(false)

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value.trim(), completed: false })
    newTodo.value = ''
  }
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  return showUnfinishedOnly.value
    ? todos.value.filter(todo => !todo.completed)
    : todos.value
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(to right top, #c1dfc4, #deecdd);
  min-height: 100vh;
}

.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
}

.todo-card {
  background: white;
  padding: 2rem;
  border-radius: 20px;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 12px 35px rgba(0, 0, 0, 0.15);
  transition: 0.3s;
}

.todo-card:hover {
  transform: scale(1.01);
}

h1 {
  text-align: center;
  color: #3a3a3a;
  margin-bottom: 1.5rem;
  font-size: 1.8rem;
}

.todo-form {
  display: flex;
  gap: 10px;
  margin-bottom: 1rem;
}

.todo-form input {
  flex: 1;
  padding: 0.8rem;
  font-size: 1rem;
  border: 2px solid #ccc;
  border-radius: 12px;
  transition: 0.3s;
}

.todo-form input:focus {
  outline: none;
  border-color: #6bdba7;
  box-shadow: 0 0 5px rgba(107, 219, 167, 0.5);
}

.todo-form button {
  padding: 0 1rem;
  background: #6bdba7;
  border: none;
  color: white;
  border-radius: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s, transform 0.2s;
}

.todo-form button:hover {
  background: #53c89f;
  transform: scale(1.05);
}

.filter {
  font-size: 0.95rem;
  margin-bottom: 1rem;
  color: #555;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  background: #f3f7f4;
  padding: 0.9rem;
  border-radius: 12px;
  margin-bottom: 0.6rem;
  transition: box-shadow 0.3s;
}

.todo-item:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.todo-item input[type='checkbox'] {
  margin-right: 0.75rem;
  transform: scale(1.2);
}

.todo-item span {
  flex: 1;
  font-size: 1rem;
  color: #333;
  transition: color 0.3s;
}

.done {
  text-decoration: line-through;
  color: #999;
}

.delete-btn {
  background: #ff6b6b;
  color: white;
  border: none;
  padding: 0.3rem 0.7rem;
  border-radius: 8px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: background 0.3s;
}

.delete-btn:hover {
  background: #e63946;
}

/* Animasi */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
