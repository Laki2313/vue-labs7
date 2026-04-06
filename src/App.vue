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
import { ref, onMounted } from 'vue'

export default {
  setup() {
    const items = ref([])
    const isLoading = ref(false)
    const error = ref(null)


    const loadItems = async () => {
      isLoading.value = true
      error.value = null

      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/posts')

        if (!response.ok) {
          throw new Error('Помилка сервера')
        }

        const data = await response.json()
        items.value = data.slice(0, 10)

      } catch (err) {
        error.value = err.message
        items.value = []
      } finally {
        isLoading.value = false
      }
    }

    onMounted(() => {
      loadItems()
    })

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

li {
  margin-bottom: 10px;
}
</style>
