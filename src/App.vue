<template>
  <div class="container">
    <h1>Cari Gambar</h1>
    <div class="search-container">
      <input type="text" v-model="searchTerm" placeholder="Masukkan kata kunci pencarian..." />
      <button @click="searchImages">Cari Gambar</button>
    </div>
    <div class="image-container">
      <img :src="imageUrl" alt="Hasil Pencarian" v-if="imageUrl" />
      <p v-if="error">{{ error }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const searchTerm = ref('')
const imageUrl = ref('')
const error = ref('')

// Method untuk mencari gambar berdasarkan kata kunci
const searchImages = async () => {
  try {
    if (!searchTerm.value.trim()) {
      alert('Masukkan kata kunci pencarian!')
      return
    }

    error.value = ''
    imageUrl.value = ''

    // Menggunakan Pixabay API untuk pencarian gambar
    const response = await fetch(
      `https://pixabay.com/api/?key=44881105-070e6e8cefc7dd7e52394ab99&q=${encodeURIComponent(searchTerm.value)}&image_type=photo`
    )

    if (response.ok) {
      const data = await response.json()
      if (data.hits.length > 0) {
        imageUrl.value = data.hits[0].webformatURL
      } else {
        error.value = 'Gambar tidak ditemukan. Coba kata kunci yang lain.'
      }
    } else {
      error.value = 'Terjadi kesalahan dalam mencari gambar.'
    }
  } catch (error) {
    console.error('Error fetching image:', error)
    error.value = 'Terjadi kesalahan saat mengambil gambar.'
  }
}
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

p {
  color: red;
  font-size: 16px;
}
</style>
