<template>
    <div class="search-section">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search users by name..."
        class="search-input"
      />
    </div>
</template>
  
<script setup lang="ts">
const searchQuery = defineModel<string>('inputValue')
</script>
  
<style scoped>
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
</style>