<template>
  <section class="about">
    <h1>О проекте</h1>
    <p>
      Это учебная библиотека книг на Vue 3. Книги можно добавлять, искать по
      названию, редактировать и удалять, а список сохраняется в браузере.
    </p>
    <p class="about__author">Автор: Алексей</p>
  </section>
</template>

<style scoped>
.about p {
  max-width: 560px;
}

.about__author {
  margin-top: 16px;
  color: var(--text-h);
  font-weight: 500;
}
</style>
