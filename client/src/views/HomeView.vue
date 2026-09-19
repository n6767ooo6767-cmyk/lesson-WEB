<template>
  <div>
    <h1>Моя библиотека</h1>
    <AddBook @add="addBook" />
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
        @update="updateBook"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import Book from '../components/Book.vue'
import AddBook from '../components/AddBook.vue'

const books = ref([])
const searchQuery = ref('')
let nextId = 1

onMounted(() => {
  const saved = localStorage.getItem('books')
  if (saved) {
    books.value = JSON.parse(saved)
    nextId = books.value.length
      ? Math.max(...books.value.map(b => b.id)) + 1
      : 1
  } else {
    books.value = [
      { id: 1, title: 'Гарри Поттер', author: 'Роулинг', genre: 'Фэнтези' },
      { id: 2, title: 'Властелин колец', author: 'Толкин', genre: 'Фэнтези' }
    ]
    nextId = 3
  }
})

watch(books, (newVal) => {
  localStorage.setItem('books', JSON.stringify(newVal))
}, { deep: true })

function addBook(bookData) {
  books.value.push({
    id: nextId++,
    title: bookData.title,
    author: bookData.author,
    genre: bookData.genre
  })
}

function removeBook(id) {
  books.value = books.value.filter(book => book.id !== id)
}

function updateBook(updatedBook) {
  const index = books.value.findIndex(b => b.id === updatedBook.id)
  if (index !== -1) {
    books.value[index] = { ...updatedBook }
  }
}

const filteredBooks = computed(() => {
  if (!searchQuery.value) return books.value
  return books.value.filter(book =>
    book.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const totalBooks = computed(() => books.value.length)
</script>
