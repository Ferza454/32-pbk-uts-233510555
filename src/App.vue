<template>
  <div class="container">
    <h1>To-Do List</h1>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Tambahkan tugas baru..." />
    <ul>
      <TodoItem
        v-for="(task, index) in tasks"
        :key="index"
        :task="task"
        @remove="removeTask(index)"
        @update="saveTasks"
      />
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import TodoItem from './components/TodoItem.vue'

const newTask = ref('')
const tasks = ref([])

function loadTasks() {
  const saved = localStorage.getItem('tasks')
  if (saved) {
    tasks.value = JSON.parse(saved)
  }
}

function saveTasks() {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

function addTask() {
  if (newTask.value.trim() !== '') {
    tasks.value.push({ text: newTask.value.trim(), done: false })
    newTask.value = ''
    saveTasks()
  }
}

function removeTask(index) {
  tasks.value.splice(index, 1)
  saveTasks()
}

onMounted(loadTasks)
</script>
