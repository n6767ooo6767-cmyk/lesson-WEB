<template>
  <article v-if="book" class="book-detail">
    <h1>{{ book.title }}</h1>
    <dl class="book-detail__meta">
      <div>
        <dt>Автор</dt>
        <dd>{{ book.author }}</dd>
      </div>
      <div v-if="book.genre">
        <dt>Жанр</dt>
        <dd>{{ book.genre }}</dd>
      </div>
    </dl>
    <router-link class="back" to="/">← Назад в библиотеку</router-link>
  </article>

  <section v-else class="book-detail">
    <h1>Книга не найдена</h1>
    <p>Возможно, она была удалена из библиотеки.</p>
    <router-link class="back" to="/">← Вернуться в библиотеку</router-link>
  </section>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()

const book = computed(() => {
  try {
    const books = JSON.parse(localStorage.getItem('books'))
    if (!Array.isArray(books)) return null
    return books.find((b) => b.id === Number(route.params.id)) ?? null
  } catch {
    return null
  }
})
</script>

<style scoped>
.book-detail {
  max-width: 560px;
}

.book-detail__meta {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 8px 16px;
  margin: 0 0 24px;
}

.book-detail__meta dt {
  color: var(--text);
  font-size: 14px;
}

.book-detail__meta dd {
  margin: 0;
  color: var(--text-h);
  font-weight: 500;
}

.book-detail .back {
  display: inline-block;
  text-decoration: none;
}

.book-detail .back:hover {
  text-decoration: underline;
}
</style>
