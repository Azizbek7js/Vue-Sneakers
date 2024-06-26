<script setup>
  defineProps({
    imageUrl: String,
    title: String,
    price:Number,
    isFavorite:Boolean,
    isAdded:Boolean,
    onClickAdd:Function,
    onClickFavorite:Function
  })
</script>

<template>
  <div>
    <div class="relative m-10 border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">
      <img @click="onClickFavorite" class="absolute top-0 left-0" :src="isFavorite ? '/liked.svg': 'unliked.svg'" alt="like" />
      <img :src="imageUrl" alt="sneakers" />
      <p class="mt-2">{{title}}</p>
      <div class="flex justify-between mt-5">
        <div class="flex flex-col">
          <span class="text-slate-400">Cost:</span>
          <b>${{price}}</b>
        </div>
        <img @click="onClickAdd" :src="!isAdded? '/plus.svg' : 'btn-checked.svg'" alt="plus" />
      </div>
    </div>

  </div>
</template>