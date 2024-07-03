<script setup>
import { inject } from 'vue';
import CardItem from '@/components/CardItem.vue';
const { cartItems, removeFromCart } = inject('cartActions');
</script>

<template>
  <div v-auto-animate class="flex flex-col flex-1 gap-4 mt-4 justify-between">
    <CardItem
        v-for="item in cartItems"
        :key="item.id"
        :title="item.title"
        @onClickRemove="() => removeFromCart(item)"
        :price="item.price"
        :image-url="item.imageUrl"
        :id="item.id"
    />
  </div>
</template>

<script>
import autoAnimate from '/autoAnimate.js';
export default {
  directives: {
    autoAnimate
  }
};
</script>
