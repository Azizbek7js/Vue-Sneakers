<script setup>
import {onMounted, reactive, ref, provide, watch, computed} from "vue";
import axios from "axios";
import Header from "./components/Header.vue"
import CardList from "@/components/CardList.vue";
import Drawer from "@/components/Drawer.vue";
const products = ref([])
const cartItems = ref([])
const isCreatingOrder = ref(false)
const favoritesProduct = ref([])
const filters = reactive({
  sortBy:'',
  searchQuery:''
})
const onchangeSelect = (event)=>{
  filters.sortBy = event.target.value
}
const onchangeSearchInput = (event)=>{
  filters.searchQuery = event.target.value
}
const drawerOpen = ref(false)
const totalPrice = computed(
    ()=> cartItems.value.reduce((acc , item)=> acc + item.price, 0)
)

const addToFavorite = async (item)=>{
   try{
     if(!item.isFavorite){
       const obj = {
         parentId:item.id
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

const closeDrawer = () =>{
  drawerOpen.value = false
}
const addToCart= (item)=>{
    cartItems.value.push(item)
    item.isAdded = true
}
const removeFromCart = (item)=>{
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}
const onclickAddCart= (item)=>{
  if(!item.isAdded){
    addToCart(item)
  }else{
    removeFromCart(item)
  }
}

const createOrder = async ()=>{
  try{
    isCreatingOrder.value = true
    const { data } = await axios.post(`https://fe22cea9257eefdb.mokky.dev/orders`,{
      items: cartItems.value,
      totalPrice: totalPrice.value
    })
    cartItems.value = []
    return data
  }catch (e){
    console.log(e)
  }finally {
    isCreatingOrder.value = false
  }
}
const cartButtonDisable = computed(()=> isCreatingOrder.value? true : !totalPrice.value)

const openDrawer = () =>{
  drawerOpen.value = true
}
const fetchFavorites = async ()=>{
  try {
    const {data: favorites} = await axios.get('https://fe22cea9257eefdb.mokky.dev/favorites')
    products.value = products.value.map(item=>{
      const favorite = favorites.find(t => t.parentId === item.id)
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

onMounted(async ()=>{
  await fetchProducts();
  await fetchFavorites();
})
watch(filters , fetchProducts)
provide('cartActions',{
  cartItems,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})
</script>
<template>
  <Drawer  v-if="drawerOpen" :total-price="totalPrice" :cartButtonDisable="cartButtonDisable" @createOrder="createOrder"/>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 ">
    <Header :total-price="totalPrice" @openDrawer="openDrawer" />
    <div class="p-10">
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
      <CardList :products="products"  @add-to-favorite="addToFavorite"  @add-to-cart="onclickAddCart"/>
    </div>
  </div>
</template>

<style scoped>

</style>
