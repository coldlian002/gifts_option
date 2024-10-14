<script setup lang="ts">
import { ref } from 'vue'

interface Contact {
  id: number;
  name: string;
  relationship: string;
  birthday: string;
  importantDate: string;
  category: string;
  status: string;
}

const contacts = ref<Contact[]>([
  { id: 1, name: '张三', relationship: '朋友', birthday: '1990-01-01', importantDate: '2023-06-15', category: '朋友', status: '需要维护' },
  { id: 2, name: '李四', relationship: '同事', birthday: '1985-05-10', importantDate: '2023-07-01', category: '工作', status: '需要拓展' },
])

const newContact = ref<Contact>({
  id: 0,
  name: '',
  relationship: '',
  birthday: '',
  importantDate: '',
  category: '',
  status: '',
})

function addContact() {
  if (newContact.value.name && newContact.value.relationship) {
    newContact.value.id = contacts.value.length + 1
    contacts.value.push({ ...newContact.value })
    newContact.value = { id: 0, name: '', relationship: '', birthday: '', importantDate: '', category: '', status: '' }
  }
}

const searchQuery = ref('')

const filteredContacts = computed(() => {
  return contacts.value.filter(contact => 
    contact.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
    contact.relationship.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>

<template>
  <div class="contact-management">
    <h2>联系人管理</h2>
    <div class="add-contact">
      <input v-model="newContact.name" placeholder="姓名" />
      <input v-model="newContact.relationship" placeholder="关系" />
      <input v-model="newContact.birthday" type="date" placeholder="生日" />
      <input v-model="newContact.importantDate" type="date" placeholder="重要日期" />
      <input v-model="newContact.category" placeholder="分类" />
      <select v-model="newContact.status">
        <option value="需要维护">需要维护</option>
        <option value="需要拓展">需要拓展</option>
        <option value="需要回礼">需要回礼</option>
        <option value="重要人物">重要人物</option>
      </select>
      <button @click="addContact">添加联系人</button>
    </div>
    <div class="search-contacts">
      <input v-model="searchQuery" placeholder="搜索联系人" />
    </div>
    <ul class="contact-list">
      <li v-for="contact in filteredContacts" :key="contact.id">
        {{ contact.name }} - {{ contact.relationship }} - {{ contact.status }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.contact-management {
  text-align: left;
}

.add-contact, .search-contacts {
  margin-bottom: 1rem;
}

input, select, button {
  margin-right: 0.5rem;
  margin-bottom: 0.5rem;
  padding: 0.5rem;
}

.contact-list {
  list-style-type: none;
  padding: 0;
}

.contact-list li {
  background-color: #f8f9fa;
  border: 1px solid #e9ecef;
  border-radius: 4px;
  padding: 0.5rem;
  margin-bottom: 0.5rem;
}
</style>