<script setup>
import Card from './Card.vue'

const onClickAdd = () => {
  alert('Add')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      imageUrl="/sneakers/sneakers-1.jpg"
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="5000"
      isFavorite="true"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
