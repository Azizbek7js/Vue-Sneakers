<script setup>
import DrawerHeader from "@/components/DrawerHeader.vue";
import CardItemList from "@/components/CardItemList.vue";
import InfoBlock from "@/components/InfoBlock.vue";
import axios from "axios";
import {inject,computed, ref} from "vue";

const props = defineProps({
  totalPrice:Number,
})

const {cartItems, closeDrawer} =  inject('cartActions')

const isCreatingOrder = ref(false)

const cartButtonDisable = computed(()=> isCreatingOrder.value? true : !props.totalPrice)


const createOrder = async ()=>{
  try{
    isCreatingOrder.value = true
    const { data } = await axios.post(`https://fe22cea9257eefdb.mokky.dev/orders`,{
      items: cartItems.value,
      totalPrice: props.totalPrice
    })
    cartItems.value = []

    return data
  }catch (e){
    console.log(e)
  }finally {
    isCreatingOrder.value = false
  }
}
</script>
<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-76 h-full fixed right-0 top-0 z-20 p-8">
     <DrawerHeader/>
    <div v-if="!totalPrice" class="h-full flex items-center">
      <InfoBlock title="Korzinka" description="description" image-url="/empty-cart.jpg" />
    </div>
    <CardItemList v-if="totalPrice"/>
    <div v-if="totalPrice" class="flex flex-col gap-4 my-7">
      <div class="flex gap-2 ">
        <span>Total:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>${{totalPrice}}</b>
      </div>
      <div class="flex gap-2 ">
        <span>tax 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>${{Math.round((totalPrice*0.05))}}</b>
      </div>
      <button
          :disabled="cartButtonDisable"
          @click="createOrder"
          class="mt-4 transition bg-lime-500 w-full disabled:bg-slate-300 cursor-pointer rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-700">buy </button>
    </div>

  </div>
</template>
