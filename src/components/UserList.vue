<script setup lang="ts">
import { useRouter } from 'vue-router'
import type { User } from '~/types/user'
import UserCard from './UserCard.vue';

interface Props {
  users: User[]
}

defineProps<Props>()

const router = useRouter()

const navigateToUser = (userId: number): void => {
  router.push(`/user/${userId}`)
}
</script>

<template>
  <div class="user-list">
    <div 
      v-for="user in users" 
      :key="user.id"
      class="user-list-container"
    >
      <UserCard 
        :user="user"
        @click="navigateToUser(user.id)"
      />
    </div>
    
    <div v-if="users.length === 0" class="no-users">
      No users found matching your search.
    </div>
  </div>
</template>

<style scoped>
.user-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  padding: 1rem 0;
}

.no-users {
  grid-column: 1 / -1;
  text-align: center;
  padding: 3rem;
  color: #7f8c8d;
  font-size: 1.1rem;
}

@media (max-width: 768px) {
  .user-list {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
}
</style>