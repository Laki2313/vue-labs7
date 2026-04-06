<template>
  <div class="app">
    <h1>Каталог постів</h1>

    <input
      v-model="query"
      placeholder="Пошук..."
      class="search"
    />

    <p v-if="isLoading">Завантаження...</p>

    <p v-else-if="error">Помилка: {{ error }}</p>

    <p v-else-if="filteredItems.length === 0">Нічого не знайдено</p>

    <ul v-else>
      <li v-for="item in filteredItems" :key="item.id">
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
import { ref, onMounted, computed } from 'vue'

export default {
  setup() {
    const items = ref([])
    const isLoading = ref(false)
    const error = ref(null)
    const selectedItem = ref(null)

    const query = ref('')

  
    const filteredItems = computed(() => {
      return items.value.filter(item =>
        item.title.toLowerCase().includes(query.value.toLowerCase())
      )
    })

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
      selectItem,
      query,
      filteredItems
    }
  }
}
</script>

<style>
.app {
  padding: 20px;
  font-family: Arial;
}

.search {
  margin-bottom: 20px;
  padding: 8px;
  width: 250px;
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
