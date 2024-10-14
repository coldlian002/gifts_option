<script setup lang="ts">
import { ref } from 'vue'

const upcomingEvents = ref([
  { name: '妈妈生日', date: '2023-06-15' },
  { name: '朋友结婚纪念日', date: '2023-06-20' },
])

const relationshipsToMaintain = ref([
  { name: '张三', status: '需要维护' },
  { name: '李四', status: '需要回礼' },
])
</script>

<template>
  <div class="dashboard">
    <h2>仪表盘</h2>
    <div class="upcoming-events">
      <h3>即将到来的重要日期</h3>
      <ul>
        <li v-for="event in upcomingEvents" :key="event.name">
          {{ event.name }} - {{ event.date }}
        </li>
      </ul>
    </div>
    <div class="relationships">
      <h3>需要维护的关系</h3>
      <ul>
        <li v-for="relation in relationshipsToMaintain" :key="relation.name">
          {{ relation.name }} - {{ relation.status }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.dashboard {
  text-align: left;
}

h2, h3 {
  color: #333;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #f8f9fa;
  border: 1px solid #e9ecef;
  border-radius: 4px;
  padding: 0.5rem;
  margin-bottom: 0.5rem;
}
</style>