<template>
  <div class="book-card" @dblclick="startEdit">
    <div v-if="!isEditing" class="info">
      <router-link :to="`/book/${book.id}`">
        <h3>{{ book.title }}</h3>
      </router-link>
      <p>Автор: {{ book.author }}</p>
      <p v-if="book.genre">Жанр: {{ book.genre }}</p>
    </div>
    <div v-else class="edit">
      <input v-model="editTitle" placeholder="Название" />
      <input v-model="editAuthor" placeholder="Автор" />
      <input v-model="editGenre" placeholder="Жанр" />
      <button @click.stop="saveEdit">Сохранить</button>
      <button @click.stop="cancelEdit">Отмена</button>
    </div>
    <button class="delete" @click.stop="emitDelete">Удалить</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  book: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['delete', 'update'])

const isEditing = ref(false)
const editTitle = ref('')
const editAuthor = ref('')
const editGenre = ref('')

function startEdit() {
  isEditing.value = true
  editTitle.value = props.book.title
  editAuthor.value = props.book.author
  editGenre.value = props.book.genre || ''
}

function saveEdit() {
  emit('update', {
    id: props.book.id,
    title: editTitle.value,
    author: editAuthor.value,
    genre: editGenre.value
  })
  isEditing.value = false
}

function cancelEdit() {
  isEditing.value = false
}

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
  cursor: pointer;
}
.book-card h3 {
  margin: 0 0 8px 0;
}
.book-card a {
  color: #2c3e50;
  text-decoration: none;
}
.book-card a:hover {
  text-decoration: underline;
}
.info {
  flex: 1;
}
.edit {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.edit input {
  padding: 6px;
  margin-bottom: 4px;
}
button {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-left: 4px;
}
.delete {
  background: #e74c3c;
  color: white;
}
.edit button {
  background: #42b883;
  color: white;
}
</style>
