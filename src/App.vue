<template>
  <div class="container">
    <h1>To-Do List</h1>

    <div class="task-input">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Tambahkan tugas baru..."
      />
      <button @click="addTask">Tambahkan Tugas</button>
    </div>

    <div class="filters">
      <button :class="{ active: filter === 'all' }" @click="filter = 'all'">Semua</button>
      <button :class="{ active: filter === 'undone' }" @click="filter = 'undone'">Belum Selesai</button>
      <button :class="{ active: filter === 'done' }" @click="filter = 'done'">Selesai</button>
    </div>

    <ul>
      <TodoItem
        v-for="(task, index) in filteredTasks"
        :key="index"
        :task="task"
        @remove="removeTask(index)"
        @update="saveTasks"
      />
    </ul>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, nextTick } from 'vue'
import TodoItem from './components/TodoItem.vue'

const newTask = ref('')
const tasks = ref([])
const filter = ref('all')

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
  const trimmed = newTask.value.trim()
  if (trimmed !== '') {
    tasks.value.push({ text: trimmed, done: false })
    newTask.value = ''
    saveTasks()
  }
}

function removeTask(index) {
  tasks.value.splice(index, 1)
  saveTasks()
}

const filteredTasks = computed(() => {
  if (filter.value === 'done') return tasks.value.filter(t => t.done)
  if (filter.value === 'undone') return tasks.value.filter(t => !t.done)
  return tasks.value
})

// Animasi tombol saat diklik
onMounted(() => {
  nextTick(() => {
    document.querySelectorAll('button').forEach(button => {
      button.addEventListener('click', () => {
        button.classList.remove('clicked') // reset dulu
        void button.offsetWidth // trigger reflow
        button.classList.add('clicked')
      })
    })
  })
})

onMounted(loadTasks)
</script>

<style scoped>
@keyframes floatClick {
  0% {
    transform: translateY(0) scale(1);
  }
  50% {
    transform: translateY(-6px) scale(1.05);
  }
  100% {
    transform: translateY(0) scale(1);
  }
}

button.clicked {
  animation: floatClick 0.4s ease;
}

.container {
  max-width: 500px;
  margin: 50px auto;
  text-align: center;
  padding: 30px;
  border-radius: 12px;
  background: linear-gradient(135deg, #6a11cb, #2575fc);
  color: white;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

h1 {
  margin-bottom: 20px;
  font-size: 2.5em;
}

.task-input {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.task-input input {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  border: none;
  border-radius: 6px;
}

.task-input button {
  padding: 10px 15px;
  font-size: 16px;
  background: #9b59b6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.task-input button:hover {
  background: #8e44ad;
}

.filters {
  margin-bottom: 20px;
}

.filters button {
  margin: 0 5px;
  padding: 6px 12px;
  cursor: pointer;
  border: none;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border-radius: 4px;
  transition: background 0.3s ease;
}

.filters button:hover {
  background: rgba(255, 255, 255, 0.2);
}

.filters button.active {
  background: white;
  color: #6a11cb;
  font-weight: bold;
}

ul {
  list-style: none;
  padding: 0;
}
</style>
