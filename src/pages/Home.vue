<script setup>
import CardList from "@/components/CardList.vue";
import axios from "axios";
import {inject, onMounted, reactive, ref, watch} from "vue";

const {cartItems , addToCart, removeFromCart}=inject('cartActions')
const products = ref([])
const filters = reactive({
  sortBy:'',
  searchQuery:''
})
const onclickAddCart= (item)=>{
  if(!item.isAdded){
    addToCart(item)
  }else{
    removeFromCart(item)
  }
}
const fetchFavorites = async ()=>{
  try {
    const {data: favorites} = await axios.get('https://fe22cea9257eefdb.mokky.dev/favorites')
    products.value = products.value.map(item=>{
      const favorite = favorites.find(t => t.item_id === item.id)
      if (!favorite){
        return item
      }
      return {
        ...item,
        isFavorite:true,
        favoriteId: favorite.id
      }
    })
  }catch (err){
    console.log(err)
  }
}
const fetchProducts = async () =>{
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery){
      params.title = `*${filters.searchQuery}*`
    }
    const {data} = await axios.get('https://fe22cea9257eefdb.mokky.dev/items', {
      params
    })
    products.value = data.map(item =>{
      return {
        ...item,
        isFavorite:false,
        favoriteId:null,
        isAdded:false
      }
    })
  }catch (e){
    console.log(e)
  }
}

const onchangeSelect = (event)=>{
  filters.sortBy = event.target.value
}
const onchangeSearchInput = (event)=>{
  filters.searchQuery = event.target.value
}
const addToFavorite = async (item)=>{
  try{
    if(!item.isFavorite){
      const obj = {
        item_id:item.id,
      }
      item.isFavorite = true
      const {data} = await axios.post('https://fe22cea9257eefdb.mokky.dev/favorites', obj)
      item.favoriteId =data.id
    }else{
      item.isFavorite = false
      await axios.delete(`https://fe22cea9257eefdb.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  }catch (e){
    console.log(e)
  }

}
watch(cartItems,()=>{
  products.value = products.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
watch(filters , fetchProducts)

onMounted(async ()=>{
  cartItems.value = JSON.parse(localStorage.getItem('cart')|| [])
  await fetchProducts();
  await fetchFavorites();
  products.value = products.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cart)=> cart.id === item.id)
  }))
})

</script>
<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold">All shoes</h2>
    <div class="flex gap-4">
      <select @change="onchangeSelect" class="py-2 border rounded-md outline-none">
        <option value="name">Name</option>
        <option value="price">Low price</option>
        <option value="-price">High price</option>
      </select>
      <div class="relative">
        <img class="absolute left-4 top-2.5" src="/search.svg" alt="search"/>
        <input
            @input="onchangeSearchInput"
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            type="text"
            placeholder="Search...."
        >
      </div>
    </div>

  </div>
  <div class="mt-10">
    <CardList :products="products"  @add-to-favorite="addToFavorite"  @add-to-cart="onclickAddCart"/>
  </div>
</template>
