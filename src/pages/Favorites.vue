<script setup>
import {onMounted, ref} from "vue";
import axios  from "axios";
import CardList from "@/components/CardList.vue";
const favorites = ref()
onMounted(async ()=>{
 try{
   const {data}= await axios.get('https://fe22cea9257eefdb.mokky.dev/favorites?_relations=items')
   favorites.value = data.map(obj => obj.item)

 }catch (err){
   console.log(err)
 }
})
</script>
<template>
  <h1 class="text-3xl font-bold mb-8">
    My Favorites
  </h1>

  <CardList :products="favorites" is-favorites />
</template>