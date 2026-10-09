<template>
  <div class="sandbox">
    <h1>Песочница</h1>
  </div>
  <p>{{ message }}</p>
  <button @click="message = 'привет'">привет</button>
  <button @click="hello">bvhgrdo;mfvln</button>

  <h3>{{ kompliment }}</h3>
  <div v-for="value in kompliments" :key="value">
    <button @click="kompliment = value">комплимент </button>
  </div>
  <div
    class="hover-box"
    :class="{active: isHovering}"
    @mouseover="isHovering = true"
    @mouseleave="isHovering = false"
  >
    {{ isHovering ? "мышь над блоком" : "мышь не над блоком" }}
  </div>
  <input
    v-model="typed"
    @keydown.enter="onEnter"
    @keydown.esc="typed = ''"
    placeholder="Набери текст. Enter — сохранить, Esc — очистить"
    :maxlength="10"
  />
  <p>Введено: {{ typed }}</p>
  <p v-if="savedText">Сохранено: {{ savedText }}</p>
</template>

<script setup>
import { ref } from "vue"
const message = ref("нажми на кнопку")
const typed = ref('')
const savedText = ref('')
function onEnter() {
  savedText.value = typed.value
}
const kompliments = ref(["ты умный", "ты крутой", "Ты крутой программист"])
function hello(){
  message.value = "привет, ты нажал на кнопку в" + new Date().toLocaleTimeString()
}
const kompliment = ref("кнопки комплименты")
const isHovering = ref(false)
</script>

<style scoped>
.sandbox {
  display: flex;
  flex-direction: column;
  gap: 16px;
  align-items: flex-start;
}
button {
  padding: 8px 16px;
  cursor: pointer;
}
.hover-box {
  padding: 24px;
  border: 2px dashed #999;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}
.hover-box.active {
  background: #1001f6;
  color: #6b71c8;
  border-color: #ff0000a0;
}

</style>

















<!-- <template>
  <header class="site-header">
    <div class="site-header__inner">
      <router-link to="/" class="brand">Моя библиотека</router-link>
      <nav class="site-nav">
        <router-link to="/">Библиотека</router-link>
        <router-link to="/about">О проекте</router-link>
      </nav>
    </div>
  </header>

  <main class="site-main">
    <router-view />
  </main>

  <footer class="site-footer">
    <p>Учебный проект на Vue 3 + Vue Router</p>
  </footer>
</template>

<script setup>
</script>

<style scoped>
.site-header {
  position: sticky;
  top: 0;
  z-index: 10;
  background: var(--bg);
  border-bottom: 1px solid var(--border);
}

.site-header__inner {
  width: min(920px, 100%);
  margin: 0 auto;
  padding: 14px 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  flex-wrap: wrap;
}

.brand {
  font-family: var(--heading);
  font-size: 20px;
  font-weight: 600;
  color: var(--text-h);
  text-decoration: none;
}

.site-nav {
  display: flex;
  gap: 8px;
}

.site-nav a {
  padding: 7px 14px;
  border-radius: 999px;
  text-decoration: none;
  color: var(--text);
  transition:
    color 0.2s,
    background-color 0.2s;
}

.site-nav a:hover {
  color: var(--text-h);
  background: var(--bg-soft);
}

.site-nav a.router-link-exact-active {
  color: var(--accent);
  background: var(--accent-bg);
}

.site-main {
  flex: 1;
  width: min(920px, 100%);
  margin: 0 auto;
  padding: 32px 20px 56px;
}

.site-footer {
  border-top: 1px solid var(--border);
}

.site-footer p {
  width: min(920px, 100%);
  margin: 0 auto;
  padding: 16px 20px;
  font-size: 14px;
  text-align: center;
}
</style>
 -->