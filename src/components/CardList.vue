<script setup>
import Card from "@/components/Card.vue";
defineProps({
  products:Array,
})
const emit = defineEmits(['addToFavorite','addToCart'])

</script>
<template>
  <div class="grid grid-cols-4 gap-8">
    <Card
        v-for="item in products"
        :image-url="item.imageUrl"
        :title="item.title"
        :id="item.id"
        :price="item.price"
        :is-favorite="item.isFavorite"
        :on-click-add="()=> emit('addToCart', item)"
        :on-click-favorite="() => emit('addToFavorite',item)"
        :key="item.id"
        :is-added="item.isAdded"
    />
  </div>
</template>
