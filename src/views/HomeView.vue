<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import type { User, UserState } from '~/types/user'
import UserList from '@/components/UserList.vue'

const users = ref<User[]>([])
const loading = ref<boolean>(true)
const error = ref<string | null>(null)
const searchQuery = ref<string>('')

const fetchUsers = async (): Promise<void> => {
  try {
    loading.value = true
    error.value = null
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    
    const data: User[] = await response.json()
    users.value = data
  } catch (err: any) {
    error.value = err.message
    console.error('Error fetching users:', err)
  } finally {
    loading.value = false
  }
}

const filteredUsers = computed((): User[] => {
  if (!searchQuery.value) {
    return users.value
  }
  
  const query = searchQuery.value.toLowerCase()
  return users.value.filter(user => 
    user.name.toLowerCase().includes(query)
  )
})

onMounted(() => {
  fetchUsers()
})
</script>

<template>
  <div class="home">
    <div class="search-section">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search users by name..."
        class="search-input"
      />
    </div>
    
    <UserList 
      :users="filteredUsers"
      v-if="!loading && !error"
    />
    
    <div v-if="loading" class="loading">
      Loading users...
    </div>
    
    <div v-if="error" class="error">
      Error loading users: {{ error }}
    </div>
  </div>
</template>

<style scoped>
.home {
  max-width: 1000px;
  margin: 0 auto;
}

.search-section {
  margin-bottom: 2rem;
}

.search-input {
  width: 100%;
  padding: 12px 16px;
  font-size: 1rem;
  border: 2px solid #e1e5e9;
  border-radius: 8px;
  outline: none;
  transition: border-color 0.3s ease;
}

.search-input:focus {
  border-color: #667eea;
}

.loading, .error {
  text-align: center;
  padding: 3rem;
  font-size: 1.1rem;
}

.error {
  color: #e74c3c;
}

.loading {
  color: #667eea;
}
</style>