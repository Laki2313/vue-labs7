<template>
  <div class="app">
    <h1>Каталог постів</h1>

    <p v-if="isLoading">Завантаження...</p>

    <p v-else-if="error">Помилка: {{ error }}</p>

    <p v-else-if="items.length === 0">Немає даних</p>

    <ul v-else>
      <li v-for="item in items" :key="item.id">
        {{ item.title }}
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    const items = ref([])      
    const isLoading = ref(false)
    const error = ref(null)

    return {
      items,
      isLoading,
      error
    }
  }
}
</script>

<style>
.app {
  padding: 20px;
  font-family: Arial;
}

h1 {
  margin-bottom: 20px;
}
</style>
