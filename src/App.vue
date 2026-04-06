<template>
  <div class="app">
    <h1>Каталог постів</h1>

    <p v-if="isLoading">Завантаження...</p>

    <p v-else-if="error">Помилка: {{ error }}</p>

    <p v-else-if="items.length === 0">Немає даних</p>

    <ul v-else>
      <li v-for="item in items" :key="item.id">
        <b>{{ item.title }}</b>
        <button @click="selectItem(item)">Деталі</button>
      </li>
    </ul>

    <div v-if="selectedItem" class="details">
      <h2>Деталі поста</h2>

      <h3>{{ selectedItem.title }}</h3>
      <p>{{ selectedItem.body }}</p>

      <button @click="selectedItem = null">Закрити</button>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  setup() {
    const items = ref([])
    const isLoading = ref(false)
    const error = ref(null)
    const selectedItem = ref(null) 

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

    const selectItem = (item) => {
      selectedItem.value = item
    }

    onMounted(() => {
      loadItems()
    })

    return {
      items,
      isLoading,
      error,
      selectedItem,
      selectItem
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

button {
  margin-left: 10px;
}

.details {
  margin-top: 30px;
  padding: 15px;
  border: 1px solid #ccc;
}
</style>
