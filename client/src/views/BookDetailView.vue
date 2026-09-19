<template>
  <div v-if="book">
    <h1>{{ book.title }}</h1>
    <p>Автор: {{ book.author }}</p>
    <p>Жанр: {{ book.genre }}</p>
    <router-link to="/">← Назад в библиотеку</router-link>
  </div>
  <div v-else>
    <p>Книга не найдена</p>
    <router-link to="/">На главную</router-link>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()

const books = JSON.parse(localStorage.getItem('books') || '[]')
const book = computed(() =>
  books.find(b => b.id === Number(route.params.id))
)
</script>
