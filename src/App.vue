<script setup lang="ts">
import { ref } from 'vue'
import Dashboard from './components/Dashboard.vue'
import ContactManagement from './components/ContactManagement.vue'
import EventReminders from './components/EventReminders.vue'
import GiftRecommendation from './components/GiftRecommendation.vue'

const currentView = ref('dashboard')

function changeView(view: string) {
  currentView.value = view
}
</script>

<template>
  <div class="container">
    <h1>社交关系管理助手</h1>
    <nav>
      <button @click="changeView('dashboard')">仪表盘</button>
      <button @click="changeView('contacts')">联系人管理</button>
      <button @click="changeView('events')">事件提醒</button>
      <button @click="changeView('gifts')">礼物推荐</button>
    </nav>
    <component :is="currentView === 'dashboard' ? Dashboard :
                   currentView === 'contacts' ? ContactManagement :
                   currentView === 'events' ? EventReminders :
                   GiftRecommendation">
    </component>
  </div>
</template>

<style>
.container {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

h1 {
  color: #333;
  font-size: 2rem;
  margin-bottom: 2rem;
}

nav {
  display: flex;
  justify-content: space-around;
  margin-bottom: 2rem;
}

nav button {
  background-color: #007aff;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

nav button:hover {
  background-color: #0056b3;
}
</style>