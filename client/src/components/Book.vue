<template>
  <div class="book-card">
    <div>
      <h3>{{ book.title }}</h3>
      <p>Автор: {{ book.author }}</p>
      <p v-if="book.genre">Жанр: {{ book.genre }}</p>
    </div>
    <button @click="emitDelete">Удалить</button>
  </div>
</template>

<script setup>
const props = defineProps({
  book: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['delete'])

function emitDelete() {
  emit('delete', props.book.id)
}
</script>

<style scoped>
.book-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin: 8px 0;
  background: #f9f9f9;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
button {
  background: #e74c3c;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}
</style>
