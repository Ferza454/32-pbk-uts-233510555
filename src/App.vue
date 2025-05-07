<template>
  <div class="container">
    <h1>To-Do List</h1>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Tambahkan tugas baru..." />
    <ul>
  <li v-for="(task, index) in tasks" :key="index">
    <label>
      <input type="checkbox" v-model="task.done" @change="saveTasks" />
      <span :class="{ done: task.done }">{{ task.text }}</span>
    </label>
    <button @click="removeTask(index)">Hapus</button>
  </li>
</ul>

  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

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


<style scoped>
.container {
  max-width: 500px;
  margin: 50px auto;
  text-align: center;
}
input {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
}
button {
  margin-left: 10px;
  color: red;
}

.done {
  text-decoration: line-through;
  color: gray;
}

</style>
