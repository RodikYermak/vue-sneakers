<script setup>
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <!-- <Drawer /> -->
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select class="py-2 px-3 border rounded-md outline-none">
            <option value="all">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="popular">По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img src="/search.svg" class="absolute left-4 top-3" />
            <input
              type="text"
              placeholder="Поиск..."
              class="border border-slate-100 rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            />
          </div>
        </div>
      </div>

      <CardList />
    </div>
  </div>
</template>

<style scoped></style>

<!-- 2:13:50 -->
