<script setup lang="ts">
import { ref } from 'vue'

interface Event {
  id: number;
  name: string;
  date: string;
  type: string;
}

const events = ref<Event[]>([
  { id: 1, name: '妈妈生日', date: '2023-06-15', type: '生日' },
  { id: 2, name: '结婚纪念日', date: '2023-07-01', type: '纪念日' },
])

const newEvent = ref<Event>({
  id: 0,
  name: '',
  date: '',
  type: '',
})

function addEvent() {
  if (newEvent.value.name && newEvent.value.date) {
    newEvent.value.id = events.value.length + 1
    events.value.push({ ...newEvent.value })
    newEvent.value = { id: 0, name: '', date: '', type: '' }
  }
}
</script>

<template>
  <div class="event-reminders">
    <h2>事件提醒</h2>
    <div class="add-event">
      <input v-model="newEvent.name" placeholder="事件名称" />
      <input v-model="newEvent.date" type="date" />
      <select v-model="newEvent.type">
        <option value="生日">生日</option>
        <option value="纪念日">纪念日</option>
        <option value="其他">其他</option>
      </select>
      <button @click="addEvent">添加事件</button>
    </div>
    <ul class="event-list">
      <li v-for="event in events" :key="event.id">
        {{ event.name }} - {{ event.date }} - {{ event.type }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.event-reminders {
  text-align: left;
}

.add-event {
  margin-bottom: 1rem;
}

input, select, button {
  margin-right: 0.5rem;
  margin-bottom: 0.5rem;
  padding: 0.5rem;
}

.event-list {
  list-style-type: none;
  padding: 0;
}

.event-list li {
  background-color: #f8f9fa;
  border: 1px solid #e9ecef;
  border-radius: 4px;
  padding: 0.5rem;
  margin-bottom: 0.5rem;
}
</style>