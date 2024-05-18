<script setup>
import axios from 'axios'
import { ref, inject, computed } from 'vue'
import DrawerHead from './DrawerHead.vue'
import InfoBlock from './InfoBlock.vue'
import CartItemList from './CartItemList.vue'



const props = defineProps({
    totalPrice: Number,
    vatPrice: Number,
})

const { cart } = inject('cart')

const isCreating = ref(false);
const orderId = ref(null)

const createOrder = async () => {
    try {
        isCreating.value = true
        const { data } = await axios.post(`https://2e00903aa7ee66b8.mokky.dev/orders`, {
            items: cart.value,
            totalPrice: props.totalPrice.value
        })

        cart.value = []


        orderId.value = data.id

    } catch (err) {
        console.log(err);
    } finally {
        isCreating.value = false
    }
}

const cartIsEmpty = computed(() => cart.value.length === 0);
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value);

</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHead />

        <div v-if="!totalPrice || orderId" class="flex h-full items-center justify-center">
            <InfoBlock v-if="orderId" title="Заказ оформлен!"
                :description="`Ваш заказ №${orderId} скоро будет передан курьерской службе доставки`"
                imageUrl="/order-success-icon.png" />
            <InfoBlock v-if="!totalPrice && !orderId" title="Ваша корзина пуста"
                description="Добавтьте пожалуйста товар" imageUrl="/package-icon.png" />
        </div>
        <div v-else>
            <CartItemList />

            <div class="flex flex-col gap-4 mt-7">
                <div class="flex gap-2">
                    <span>Итого:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ totalPrice }} KZT</b>
                </div>

                <div class="flex gap-2">
                    <span>Налог 15%:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ vatPrice }} KZT</b>
                </div>

                <button :disabled="buttonDisabled" @click="createOrder"
                    class=" mt-4 transition bg-lime-500 w-full text-white py-3 disabled:bg-slate-300 rounded-xl hover:bg-lime-600 active:bg-lime-700 cursor-pointer ">Оформить
                    заказ</button>
            </div>
        </div>

    </div>


</template>