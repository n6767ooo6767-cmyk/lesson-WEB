<template>
  <article
    class="book-card"
    title="Двойной клик — редактировать"
    @dblclick="startEdit"
  >
    <template v-if="!isEditing">
      <div class="book-info">
        <h3 class="book-title">
          <router-link :to="`/book/${book.id}`">{{ book.title }}</router-link>
        </h3>
        <p class="book-author">{{ book.author }}</p>
      </div>
      <span v-if="book.genre" class="book-genre">{{ book.genre }}</span>
      <button class="danger" @click.stop="emitDelete">Удалить</button>
    </template>

    <form v-else class="book-edit" @submit.prevent="saveEdit" @keydown.esc="cancelEdit">
      <input v-model="editTitle" placeholder="Название" required />
      <input v-model="editAuthor" placeholder="Автор" required />
      <input v-model="editGenre" placeholder="Жанр" />
      <div class="book-edit__actions">
        <button type="submit">Сохранить</button>
        <button type="button" class="ghost" @click.stop="cancelEdit">
          Отмена
        </button>
      </div>
    </form>
  </article>
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
  editGenre.value = props.book.genre === 'Без жанра' ? '' : props.book.genre || ''
}

function saveEdit() {
  const title = editTitle.value.trim()
  const author = editAuthor.value.trim()
  if (!title || !author) return
  emit('update', {
    id: props.book.id,
    title,
    author,
    genre: editGenre.value.trim() || 'Без жанра'
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
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 14px 16px;
  border: 1px solid var(--border);
  border-radius: 12px;
  background: var(--bg);
  cursor: pointer;
  transition:
    border-color 0.2s,
    box-shadow 0.2s;
}

.book-card:hover {
  border-color: var(--accent-border);
  box-shadow: var(--shadow);
}

.book-info {
  flex: 1;
  min-width: 0;
}

.book-title {
  margin: 0;
  font-family: var(--heading);
  font-size: 17px;
  font-weight: 600;
  line-height: 130%;
}

.book-title a {
  color: var(--text-h);
  text-decoration: none;
}

.book-title a:hover {
  color: var(--accent);
  text-decoration: underline;
}

.book-author {
  margin-top: 2px;
  font-size: 14px;
}

.book-genre {
  padding: 4px 10px;
  border-radius: 999px;
  font-size: 13px;
  color: var(--accent);
  background: var(--accent-bg);
  white-space: nowrap;
}

.book-edit {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  cursor: default;
}

.book-edit input {
  flex: 1 1 140px;
  min-width: 0;
}

.book-edit__actions {
  display: flex;
  gap: 8px;
  margin-left: auto;
}
</style>
