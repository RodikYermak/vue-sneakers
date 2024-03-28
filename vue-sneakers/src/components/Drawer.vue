<script setup>
import CartItem from './CartItem.vue'
import InfoBlock from './InfoBlock.vue'
import { inject } from 'vue'

const { closeDrawer, cart, removeFromCart, totalPrice, createOrder } = inject('cart')

defineProps({
  vatPrice: Number
})
</script>

<template>
  <div class="fixed z-10 top-0 left-0 h-full w-full bg-black opacity-80">
    <div
      class="flex flex-col justify-between fixed h-full z-20 right-0 top-0 h-full right-0 w-96 bg-white px-10 py-7"
    >
      <h2 class="text-2xl font-bold mb-10 flex items-center gap-5">
        <svg
          class="rotate-180 hover:-translate-x-1 opacity-30 hover:opacity-100 transition cursor-pointer"
          width="16"
          height="14"
          viewBox="0 0 16 14"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          @click="closeDrawer"
        >
          <path
            d="M1 7H14.7143"
            stroke="black"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
          <path
            d="M8.71436 1L14.7144 7L8.71436 13"
            stroke="black"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
        Корзина
      </h2>

      <div v-if="!cart.length" class="flex h-full items-center">
        <InfoBlock
          title="Корзина пустая"
          description="Добавить хотя бы одну пару кроссовок, чтобы сделать заказ."
          imageUrl="/package-icon.png"
        />
      </div>

      <div class="flex flex-col flex-1 gap-4 justify-between">
        <div v-if="cart.length" class="flex flex-col gap-5" v-auto-animate>
          <CartItem
            v-for="item in cart"
            :key="item.id"
            :img="item.imageUrl"
            :title="item.title"
            :price="item.price"
            :onClickRemove="() => removeFromCart(item)"
          />
        </div>

        <div v-if="cart.length">
          <div class="flex flex-col gap-5">
            <div class="flex items-end gap-2">
              <span>Итого:</span>
              <div class="flex-1 border-b border-dashed"></div>
              <span class="font-bold">{{ totalPrice }} руб.</span>
            </div>

            <div class="flex items-end gap-2">
              <span>Налог 5%:</span>
              <div class="flex-1 border-b border-dashed"></div>
              <span class="font-bold">{{ vatPrice }} руб.</span>
            </div>
          </div>

          <button
            @click="createOrder"
            class="flex justify-center items-center gap-3 w-full py-3 mt-10 bg-lime-500 text-white rounded-xl transition active:bg-lime-700 hover:bg-lime-600"
          >
            Оформить заказ
            <img src="/arrow-next.svg" alt="Arrow" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
