<template>
  <li class="todo-item">
    <label>
      <input type="checkbox" v-model="task.done" @change="$emit('update')" />
      <span :class="{ done: task.done }">{{ task.text }}</span>
    </label>
    <button @click="$emit('remove')">✕</button>
  </li>
</template>

<script setup>
defineProps(['task'])
defineEmits(['remove', 'update'])
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 6px;
  padding: 10px;
  margin-bottom: 10px;
  color: white;
}

.todo-item input[type="checkbox"] {
  margin-right: 10px;
}

.todo-item .done {
  text-decoration: line-through;
  color: #d3d3d3;
}

.todo-item button {
  background: transparent;
  border: none;
  color: #ff6b6b;
  font-size: 16px;
  cursor: pointer;
  transition: color 0.3s;
}

.todo-item button:hover {
  color: #ff3b3b;
}
</style>
