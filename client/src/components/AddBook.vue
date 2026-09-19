<template>
  <form @submit.prevent="submitBook">
    <input v-model="title" placeholder="Название" required />
    <input v-model="author" placeholder="Автор" required />
    <input v-model="genre" placeholder="Жанр" />
    <button type="submit">Добавить книгу</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const title = ref('')
const author = ref('')
const genre = ref('')

const emit = defineEmits(['add'])

function submitBook() {
  if (title.value && author.value) {
    emit('add', {
      title: title.value,
      author: author.value,
      genre: genre.value || 'Без жанра'
    })
    title.value = ''
    author.value = ''
    genre.value = ''
  }
}
</script>

<style scoped>
input {
  margin: 5px;
  padding: 8px;
}
button {
  margin: 5px;
  padding: 8px 16px;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
