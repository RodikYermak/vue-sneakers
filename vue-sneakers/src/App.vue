<script setup>
import { onMounted, ref, watch, reactive, provide } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
// import Drawer from './components/Drawer.vue'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChange = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearch = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://d348e7c385bb7fd6.mokky.dev/favorites')

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })

    console.log(items)
  } catch (error) {
    console.log(error)
  }
}

const addToFavorite = async (item) => {
  try {
    item.isFavorite = !item.isFavorite
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }

      const { data } = await axios.post('https://d348e7c385bb7fd6.mokky.dev/favorites', obj)

      console.log(data)

      item.isFavorite = true
      item.favoriteId = data.id
    } else {
      await axios.delete(`https://d348e7c385bb7fd6.mokky.dev/favorites/${item.favoriteId}`)

      item.favoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://d348e7c385bb7fd6.mokky.dev/items', { params })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})
watch(filters, fetchItems)

provide('addToFavorite', addToFavorite)
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <!-- <Drawer /> -->
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select @change="onChange" class="py-2 px-3 border rounded-md outline-none">
            <option value="title">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img src="/search.svg" class="absolute left-4 top-3" />
            <input
              @input="onChangeSearch"
              type="text"
              placeholder="Поиск..."
              class="border border-slate-100 rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            />
          </div>
        </div>
      </div>
      <div class="mt-10">
        <CardList :items="items" @addToFavorite="addToFavorite" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>

<!-- 4:49:47 -->
