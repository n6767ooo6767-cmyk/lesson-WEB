<template>
  <div>
    <h1>Моя библиотека</h1>

    <div>
      <input v-model="newTitle" placeholder="Название" required />
      <input v-model="newAuthor" placeholder="Автор" required />
      <input v-model="newGenre" placeholder="Жанр" />
      <button @click="addBook">Добавить книгу</button>
    </div>

    <div>
      <input v-model="searchQuery" placeholder="Поиск по названию..." />
      <p>Всего книг: {{ totalBooks }}</p>
    </div>

    <div v-if="filteredBooks.length === 0">Книг пока нет</div>
    <div v-else>
      <Book
        v-for="book in filteredBooks"
        :key="book.id"
        :book="book"
        @delete="removeBook"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import Book from './components/Book.vue'  // если у вас BookCard.vue – измените имя

const books = ref([
  { id: 1, title: 'Гарри Поттер', author: 'Роулинг', genre: 'Фэнтези' },
  { id: 2, title: 'Властелин колец', author: 'Толкин', genre: 'Фэнтези' }
])

let nextId = 3

const newTitle = ref('')
const newAuthor = ref('')
const newGenre = ref('')

const searchQuery = ref('')

function addBook() {
  if (newTitle.value && newAuthor.value) {
    books.value.push({
      id: nextId++,
      title: newTitle.value,
      author: newAuthor.value,
      genre: newGenre.value || 'Без жанра'
    })
    newTitle.value = ''
    newAuthor.value = ''
    newGenre.value = ''
  }
}

function removeBook(id) {
  books.value = books.value.filter(book => book.id !== id)
}

const filteredBooks = computed(() => {
  if (!searchQuery.value) return books.value
  return books.value.filter(book =>
    book.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const totalBooks = computed(() => books.value.length)
</script>

<style scoped>
body { font-family: Arial, sans-serif; }
input { margin: 5px; padding: 8px; }
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
