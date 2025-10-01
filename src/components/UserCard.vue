<script setup lang="ts">
import { computed } from 'vue'
import { RouterLink } from 'vue-router'
import type { User } from '~/types/user'

interface Props {
  user: User
}

const props = defineProps<Props>()

const userInitial = computed((): string => {
  return props.user.name.charAt(0).toUpperCase()
})
</script>

<template>
  <RouterLink :to="`/user/${user.id}`" class="user-card">
    <div class="user-avatar">
      {{ userInitial }}
    </div>
    <div class="user-info">
      <h3 class="user-name">{{ user.name }}</h3>
      <p class="user-email">
        <span class="label">Email:</span> {{ user.email }}
      </p>
      <p class="user-city">
        <span class="label">City:</span> {{ user.address.city }}
      </p>
    </div>
  </RouterLink>
</template>

<style scoped>
.user-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 1rem;
  border: 2px solid transparent;
  text-decoration: none;
  color: inherit;
}

.user-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.15);
  border-color: #667eea;
}

.user-avatar {
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
  font-size: 1.2rem;
  flex-shrink: 0;
}

.user-info {
  flex: 1;
}

.user-name {
  font-size: 1.2rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #2c3e50;
}

.user-email, .user-city {
  margin-bottom: 0.25rem;
  color: #555;
}

.label {
  font-weight: 500;
  color: #7f8c8d;
}
</style>