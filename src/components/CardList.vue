<script setup>
import Card from './Card.vue'

defineProps({
    items: Array,
    isFavorites: Boolean
})

const emit = defineEmits(['addToFavorite', 'addToCart']);
</script>


<template>
    <div class="grid grid-cols-4 gap-5" v-auto-animate>
        <Card v-for="item in items" :key="item.id" :title="item.title" :image-url="item.imageUrl" :price="item.price"
            :is-added="false" :isFavorite="item.isFavorite" :id="item.id"
            :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
            :onClickAdd="isFavorites ? null : () => emit('addToCart', item)" :isAdded="item.isAdded" />

    </div>
</template>