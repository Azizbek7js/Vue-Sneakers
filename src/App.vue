<script setup>
import {onMounted, ref} from "vue";
import axios from "axios";
import Header from "./components/Header.vue"
import CardList from "@/components/CardList.vue";
import Drawer from "@/components/Drawer.vue";

const products = ref([])

onMounted(async ()=>{
  try{
    const {data} = await axios.get('https://fe22cea9257eefdb.mokky.dev/items')
    products.value = data
  }catch (e){
  }
})
</script>
<template>
<!--  <Drawer />-->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 ">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold">All shoes</h2>
        <div class="flex gap-4">
          <select class="py-2 px-3 border rounded-md outline-none">
            <option>Name</option>
            <option>Cost</option>
          </select>
          <div class="relative">
            <img class="absolute left-4 top-2.5" src="/search.svg" alt="search"/>
            <input
                class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                type="text"
                placeholder="Search...."
            >
          </div>
        </div>

      </div>
      <CardList :products="products"/>
    </div>
  </div>
</template>

<style scoped>

</style>
