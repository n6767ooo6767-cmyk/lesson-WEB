<template>
  <form class="panel add-form" @submit.prevent="submitBook">
    <input v-model="title" placeholder="Название" aria-label="Название" required />
    <input v-model="author" placeholder="Автор" aria-label="Автор" required />
    <input v-model="genre" placeholder="Жанр" aria-label="Жанр" />
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
  const trimmedTitle = title.value.trim()
  const trimmedAuthor = author.value.trim()
  if (!trimmedTitle || !trimmedAuthor) return
  emit('add', {
    title: trimmedTitle,
    author: trimmedAuthor,
    genre: genre.value.trim() || 'Без жанра'
  })
  title.value = ''
  author.value = ''
  genre.value = ''
}
</script>

<style scoped>
.add-form {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.add-form input {
  flex: 1 1 160px;
  min-width: 0;
}

.add-form button {
  flex-shrink: 0;
}
</style>
