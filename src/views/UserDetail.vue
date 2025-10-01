<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import type { User } from '~/types/user'

interface Props {
  id: string | number
}

const props = defineProps<Props>()

const user = ref<User | null>(null)
const loading = ref<boolean>(true)
const error = ref<string | null>(null)
const router = useRouter()
const route = useRoute()

// Compute user initial for avatar
const userInitial = computed((): string => {
  return user.value ? user.value.name.charAt(0).toUpperCase() : ''
})

// Fetch user details
const fetchUser = async (): Promise<void> => {
  try {
    loading.value = true
    error.value = null
    
    // Use the ID from props (passed via router) or from route params
    const userId = props.id || route.params.id
    
    const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
    
    if (!response.ok) {
      throw new Error(`User not found (Status: ${response.status})`)
    }
    
    const data: User = await response.json()
    user.value = data
  } catch (err: any) {
    error.value = err.message
    console.error('Error fetching user:', err)
  } finally {
    loading.value = false
  }
}

const goBack = (): void => {
  router.push('/')
}

onMounted(() => {
  fetchUser()
})
</script>

<template>
  <div class="user-detail">
    <div v-if="loading" class="loading">
      Loading user details...
    </div>
    
    <div v-else-if="error" class="error">
      Error: {{ error }}
    </div>
    
    <div v-else-if="user" class="user-profile">
      <button @click="goBack" class="back-button">
        ← Back to Users
      </button>
      
      <div class="profile-card">
        <div class="profile-header">
          <div class="profile-avatar">
            {{ userInitial }}
          </div>
          <div class="profile-info">
            <h1 class="profile-name">{{ user.name }}</h1>
            <p class="profile-email">{{ user.email }}</p>
          </div>
        </div>
        
        <div class="profile-details">
          <div class="detail-section">
            <h3>Contact Information</h3>
            <div class="detail-grid">
              <div class="detail-item">
                <span class="detail-label">Phone:</span>
                <span class="detail-value">{{ user.phone }}</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Website:</span>
                <span class="detail-value">{{ user.website }}</span>
              </div>
            </div>
          </div>
          
          <div class="detail-section">
            <h3>Address</h3>
            <div class="detail-grid">
              <div class="detail-item">
                <span class="detail-label">Street:</span>
                <span class="detail-value">{{ user.address.street }}</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">City:</span>
                <span class="detail-value">{{ user.address.city }}</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Zipcode:</span>
                <span class="detail-value">{{ user.address.zipcode }}</span>
              </div>
            </div>
          </div>
          
          <div class="detail-section">
            <h3>Company</h3>
            <div class="detail-grid">
              <div class="detail-item">
                <span class="detail-label">Name:</span>
                <span class="detail-value">{{ user.company.name }}</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Business:</span>
                <span class="detail-value">{{ user.company.bs }}</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Catchphrase:</span>
                <span class="detail-value">"{{ user.company.catchPhrase }}"</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <div v-else class="not-found">
      User not found
    </div>
  </div>
</template>

<style scoped>
.user-detail {
  max-width: 600px;
  margin: 0 auto;
}

.back-button {
  background: #667eea;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  margin-bottom: 2rem;
  transition: background-color 0.3s ease;
}

.back-button:hover {
  background: #5a6fd8;
}

.profile-card {
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.profile-header {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  margin-bottom: 2rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid #e1e5e9;
}

.profile-avatar {
  width: 80px;
  height: 80px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
  font-size: 2rem;
  flex-shrink: 0;
}

.profile-name {
  font-size: 1.8rem;
  font-weight: 600;
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

.profile-email {
  font-size: 1.1rem;
  color: #667eea;
}

.detail-section {
  margin-bottom: 2rem;
}

.detail-section h3 {
  color: #2c3e50;
  margin-bottom: 1rem;
  font-size: 1.2rem;
  border-left: 4px solid #667eea;
  padding-left: 0.75rem;
}

.detail-grid {
  display: grid;
  gap: 0.75rem;
}

.detail-item {
  display: flex;
  justify-content: space-between;
  padding: 0.75rem;
  background: #f8f9fa;
  border-radius: 6px;
}

.detail-label {
  font-weight: 600;
  color: #555;
}

.detail-value {
  color: #2c3e50;
}

.loading, .error, .not-found {
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

@media (max-width: 768px) {
  .profile-header {
    flex-direction: column;
    text-align: center;
  }
  
  .detail-item {
    flex-direction: column;
    gap: 0.25rem;
  }
}
</style>