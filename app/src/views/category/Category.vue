<template>
  <div>
    <h1>Categories</h1>
    <!-- Display a loading message while categories are being fetched -->
    <div v-if="loading">Loading categories...</div>

    <!-- When the categories are done loading -->
    <div v-else>
    <!-- If there are no categories available, show a friendly message -->
      <div v-if="categories.length === 0">
        No categories found. Please check back later.
      </div>

      <!-- If they are available, show them -->
      <div v-else class="category-list">
        <div
          v-for="(category, index) in categories"
          :key="index"
          class="category-card"
        >
          <h2 class="category-title">{{ category.name }}</h2>
          <p class="category-description">{{ category.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

const categories = ref<any[]>([]) 
const loading = ref<boolean>(false)

// Function to retrieve category data from the .NET API
const loadCategories = async () => {
  try {
    loading.value = true
    const response = await fetch('https://localhost:5001/api/categories') 
    if (!response.ok) {
      throw new Error(`Error: ${response.status} ${response.statusText}`)
    }
    const data = await response.json()
    categories.value = data
  } catch (error) {
    console.error(error)
    // Optionally display an error message or toast
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  loadCategories()
})
</script>

<style src="../scss/category.scss" scopedscoped>

</style>
