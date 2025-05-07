<script setup>
import { ref } from 'vue';

// Data reactive
const tugasBaru = ref('');
const daftarTugas = ref([]);

// Fungsi tambah tugas
const tambahTugas = () => {
  if (tugasBaru.value.trim() === '') return;
  
  daftarTugas.value.push({
    teks: tugasBaru.value,
    selesai: false
  });
  
  tugasBaru.value = '';
};
</script>

<template>
  <div class="app">
    <h1>Daftar Tugas</h1>
    
    <div class="input-area">
      <input
        type="text"
        v-model="tugasBaru"
        placeholder="Tulis tugas..."
        @keyup.enter="tambahTugas"
      />
      <button @click="tambahTugas">Tambah</button>
    </div>
    
    <div class="daftar-tugas">
      <div v-for="(tugas, index) in daftarTugas" :key="index">
        {{ tugas.teks }}
      </div>
    </div>
  </div>
</template>

<style>
.app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}
</style>