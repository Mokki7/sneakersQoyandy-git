<script setup>


defineProps({
    id: Number,
    title: String,
    imageUrl: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onClickFavorite: Function,
    onClickAdd: Function
})



</script>

<template>
    <div
        class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition">
        <img v-if="onClickFavorite" :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="Like 1"
            :class="{ 'border-slate-200': !isFavorite, 'border-red-300': isFavorite, 'border-4': true, 'rounded-full': true, 'absolute': true, 'top-8': true, 'left-8': true }"
            @click="onClickFavorite">
        <img :src="imageUrl" alt="Sneaker">
        <p class="mt-2">{{ title }}</p>
        <div class="flex justify-between mt-5">
            <div class="flex flex-col">
                <span class="text-slate-400">Цена:</span>
                <b>{{ price }} KZT.</b>
            </div>
            <img v-if="onClickAdd" @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt=" Plus">
        </div>
    </div>
</template>