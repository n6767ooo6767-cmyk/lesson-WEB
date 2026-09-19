<template>
  <section class="library">
    <h1>Моя библиотека</h1>

    <AddBook @add="addBook" />

    <div class="toolbar">
      <input
        v-model="searchQuery"
        type="search"
        placeholder="Поиск по названию..."
      />
      <p v-if="searchQuery" class="toolbar__counter">
        Найдено книг: {{ filteredBooks.length }}
      </p>
      <p v-else class="toolbar__counter">Всего книг: {{ totalBooks }}</p>
    </div>

    <div v-if="filteredBooks.length === 0" class="empty">
      <p v-if="totalBooks === 0">В библиотеке пока нет книг</p>
      <p v-else>По запросу «{{ searchQuery }}» ничего не найдено</p>
    </div>

    <ul v-else class="book-list">
      <li v-for="book in filteredBooks" :key="book.id">
        <Book :book="book" @delete="removeBook" @update="updateBook" />
      </li>
    </ul>
  </section>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import Book from '../components/Book.vue'
import AddBook from '../components/AddBook.vue'

const STORAGE_KEY = 'books'

const defaultBooks = [
  { id: 1, title: 'Гарри Поттер', author: 'Роулинг', genre: 'Фэнтези' },
  { id: 2, title: 'Властелин колец', author: 'Толкин', genre: 'Фэнтези' }
]

function loadBooks() {
  try {
    const saved = JSON.parse(localStorage.getItem(STORAGE_KEY))
    if (Array.isArray(saved)) return saved
  } catch {
    return null
  }
  return null
}

const books = ref(loadBooks() ?? defaultBooks)
const searchQuery = ref('')

let nextId = books.value.reduce((max, book) => Math.max(max, book.id), 0) + 1

watch(
  books,
  (newVal) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newVal))
  },
  { deep: true }
)

function addBook(bookData) {
  books.value.push({
    id: nextId++,
    title: bookData.title,
    author: bookData.author,
    genre: bookData.genre
  })
}

function removeBook(id) {
  books.value = books.value.filter((book) => book.id !== id)
}

function updateBook(updatedBook) {
  const index = books.value.findIndex((book) => book.id === updatedBook.id)
  if (index !== -1) {
    books.value[index] = { ...updatedBook }
  }
}

const filteredBooks = computed(() => {
  if (!searchQuery.value) return books.value
  const query = searchQuery.value.toLowerCase()
  return books.value.filter((book) =>
    book.title.toLowerCase().includes(query)
  )
})

const totalBooks = computed(() => books.value.length)
</script>

<style scoped>
.library {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.toolbar {
  display: flex;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
}

.toolbar input {
  flex: 1 1 220px;
}

.toolbar__counter {
  margin-left: auto;
  font-size: 14px;
}

.book-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  gap: 10px;
}

.empty {
  text-align: center;
  padding: 40px 16px;
  border: 1px dashed var(--border);
  border-radius: 12px;
  color: var(--text);
}
</style>
