<script setup>
defineProps({
  totalPrice:Number
})
   const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img class="w-10" src="/logo.png" alt="logo">
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-400">Online shop</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <li @click="() => emit('openDrawer')" class="flex items-center gap-3 hover: text-black cursor-pointer">
        <img src="/cart.svg" alt="cart">
        <b>${{totalPrice}}</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center gap-3 hover: text-black cursor-pointer">
          <img src="/heart.svg" alt="heart">
          <b>Likes</b>
        </li>
      </router-link>
      <li class="flex items-center gap-3 hover: text-black cursor-pointer">
        <img src="/user.svg" alt="profile">
        <b>Profile</b>
      </li>
    </ul>
  </header>
</template>