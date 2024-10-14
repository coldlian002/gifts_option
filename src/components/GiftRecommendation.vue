<script setup lang="ts">
import { ref } from 'vue'
import { zhipuConfig } from '../config'

const recipient = ref('')
const occasion = ref('')
const budget = ref('')
const recommendations = ref<string[]>([])
const loading = ref(false)

async function getRecommendations() {
  if (!recipient.value || !occasion.value || !budget.value) {
    alert('请填写所有字段')
    return
  }

  loading.value = true
  try {
    const response = await fetch(zhipuConfig.flashApiEndpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${zhipuConfig.apiKey}`
      },
      body: JSON.stringify({
        model: zhipuConfig.modelName,
        messages: [
          { role: 'system', content: '你是一个AI礼物推荐助手。' },
          { role: 'user', content: `为${recipient.value}推荐3个${occasion.value}礼物，预算为${budget.value}元。` }
        ]
      })
    })

    const data = await response.json()
    recommendations.value = data.choices[0].message.content.split('\n').filter((item: string) => item.trim() !== '')
  } catch (error) {
    console.error('获取推荐时出错:', error)
    alert('获取推荐失败。请重试。')
  } finally {
    loading.value = false
  }
}

function saveRecommendation(recommendation: string) {
  // 这里可以实现保存功能，例如将推荐保存到本地存储或发送到服务器
  alert(`已保存推荐: ${recommendation}`)
}

function shareRecommendation(recommendation: string) {
  // 这里可以实现分享功能，例如复制到剪贴板或打开分享对话框
  alert(`分享推荐: ${recommendation}`)
}
</script>

<template>
  <div class="gift-recommendation">
    <h2>AI 礼物推荐</h2>
    <div class="input-group">
      <input v-model="recipient" placeholder="收礼人 (例如：妈妈、朋友)" />
      <input v-model="occasion" placeholder="场合 (例如：生日、圣诞节)" />
      <input v-model="budget" placeholder="预算" type="number" />
      <button @click="getRecommendations" :disabled="loading">
        {{ loading ? '正在获取推荐...' : '获取礼物建议' }}
      </button>
    </div>
    <div v-if="recommendations.length > 0" class="recommendations">
      <h3>礼物推荐</h3>
      <ul>
        <li v-for="(recommendation, index) in recommendations" :key="index">
          {{ recommendation }}
          <button @click="saveRecommendation(recommendation)">保存</button>
          <button @click="shareRecommendation(recommendation)">分享</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.gift-recommendation {
  text-align: left;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 2rem;
}

input, button {
  padding: 0.8rem;
  font-size: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
}

button {
  background-color: #007aff;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.recommendations {
  margin-top: 2rem;
}

.recommendations ul {
  list-style-type: none;
  padding: 0;
}

.recommendations li {
  background-color: #f8f9fa;
  border: 1px solid #e9ecef;
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 1rem;
  font-size: 1rem;
  color: #495057;
}

.recommendations button {
  margin-left: 0.5rem;
  padding: 0.3rem 0.6rem;
  font-size: 0.8rem;
}
</style>