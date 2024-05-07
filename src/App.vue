<template>
  <div class="container">
    <h1>Cari Gambar</h1>
    <div class="search-container">
      <input type="text" v-model="searchTerm" placeholder="Masukkan kata kunci pencarian...">
      <button @click="searchImages">Cari Gambar</button>
    </div>
    <div class="image-container">
      <img :src="imageUrl" alt="Hasil Pencarian" v-if="imageUrl" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const searchTerm = ref('');
const imageUrl = ref('');

// Method untuk mencari gambar berdasarkan kata kunci
const searchImages = async () => {
  try {
    if (!searchTerm.value.trim()) {
      alert('Masukkan kata kunci pencarian!');
      return;
    }

    const response = await fetch(`https://source.unsplash.com/400x300/?${searchTerm.value}`); // Mengambil gambar dari Unsplash API
    imageUrl.value = response.url;
  } catch (error) {
    console.error('Error fetching image:', error);
  }
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.search-container {
  margin-bottom: 20px;
}

input {
  padding: 10px;
  font-size: 16px;
  border: 2px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.image-container {
  margin-top: 20px;
}

img {
  max-width: 100%;
  height: auto;
}
</style>
