<script setup>
import Card from "@/components/Card.vue";
defineProps({
  products:Array
})
 const onClickAdd = () =>{
   alert(111)
 }
</script>
<template>
  <div class="grid grid-cols-4 gap-8">
    <Card
        v-for="item in products"
        :image-url="item.imageUrl"
        :title="item.name"
        :price="item.price"
        :is-added="true"
        :is-favorite="true"
        :on-click-add="onClickAdd"
        :key="item.id"
    />
  </div>
</template>
