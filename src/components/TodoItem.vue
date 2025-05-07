<template>
  <li>
    <label>
      <input type="checkbox" v-model="task.done" @change="$emit('update')" />
      <span :class="{ done: task.done }">{{ task.text }}</span>
    </label>
    <button @click="$emit('remove')">Hapus</button>
  </li>
</template>

<script setup>
defineProps(['task'])
defineEmits(['remove', 'update'])
</script>

<style scoped>
.done {
  text-decoration: line-through;
  color: gray;
}
</style>
