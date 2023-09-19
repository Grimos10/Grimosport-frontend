<template>
    <div class="page-cart">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Carrello</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth" v-if="cartTotalLenght">
                    <thead>
                        <tr>
                            <th>Prodotto</th>
                            <th>Prezzo</th>
                            <th>Quantità</th>
                            <th>Totale</th>
                            <th></th>
                        </tr>
                    </thead>

                    <tbody>
                        <CartItem 
                            v-for="item in cart.items" 
                            :key="item.product.id" 
                            :initialItem="item"
                            v-on:removeFromCart="removeFromCart" />
                    </tbody>
                </table>

                <div class="has-text-centered" v-else>
                    <p>Il tuo carrello è vuoto.</p>
                </div>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Totale</h2>

                <strong>{{ cartTotalPrice.toFixed(2) }}€</strong>, {{ cartTotalLenght }} prodotti nel carrello.

                <hr>

                <router-link to="/cart/checkout" class="button is-dark">Procedi al checkout </router-link>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { useStore } from 'vuex';
import { ref, computed, onMounted } from 'vue';
import CartItem from '@/components/CartItem.vue';

export default {
    name: 'Cart',
    components: {
        CartItem,
    },
    setup() {
        const cart = ref({ items: [] });
        const store = useStore();

        function removeFromCart(item) {
            cart.value.items = cart.value.items.filter(i => i.product.id !== item.product.id);
        }

        onMounted(async () => {
            cart.value = store.state.cart;
        });

        const cartTotalLenght = computed(() => {
            return cart.value.items.reduce((acc, curVal) => {
                return acc += curVal.quantity;
            }, 0);
        });

        const cartTotalPrice = computed(() => {
            return cart.value.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity;
            }, 0);
        });

        return {
            cart,
            cartTotalLenght,
            cartTotalPrice,
            removeFromCart,
        };
    },
}  

</script>