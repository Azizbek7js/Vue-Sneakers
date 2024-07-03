<script setup>
import Card from "@/components/Card.vue";
defineProps({
  products:Array,
  isFavorites:Boolean
})
const emit = defineEmits(['addToFavorite','addToCart'])

</script>
<template>
  <div class="flex flex-wrap gap-8"  v-auto-animate >
    <Card
        style="height: 350px; width: 350px"
        v-for="item in products"
        :image-url="item.imageUrl"
        :title="item.title"
        :id="item.id"
        :price="item.price"
        :is-favorite="item.isFavorite"
        :on-click-add="isFavorites ? null :()=> emit('addToCart', item)"
        :on-click-favorite="isFavorites ? null :() => emit('addToFavorite',item)"
        :key="item.id"
        :is-added="item.isAdded"
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
