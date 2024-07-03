<script setup>
 defineProps({
    id:Number,
    imageUrl: String,
    title: String,
    price:Number,
    isFavorite:Boolean,
    isAdded:Boolean,
    onClickAdd:Function,
    onClickFavorite:Function,
  })
</script>

<template>
    <div class="flex flex-col justify-between relative m-10 border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">
      <img v-if="onClickFavorite" @click="onClickFavorite" class="absolute top-2 left-2" :src="isFavorite ? '/liked.svg': 'unliked.svg'" alt="like" />
      <div class="flex items-center justify-center">
        <img style="width: 200px" :src="imageUrl" alt="sneakers" />
      </div>
      <div >
        <p class="mt-2">{{title}}</p>
        <div class="flex justify-between mt-5">
          <div class="flex flex-col">
            <span class="text-slate-400">Cost:</span>
            <b>${{price}}</b>
          </div>
          <img v-if="onClickAdd" @click="onClickAdd" :src="!isAdded? '/plus.svg' : 'btn-checked.svg'" alt="plus" />
        </div>
      </div>
    </div>
</template>