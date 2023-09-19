<template>
     <tr>
        <td><router-link :to="item.product.get_absolute_url">{{ item.product.name }}</router-link></td>
        <td>{{ item.product.price }} €</td>
        <td>
            {{ item.quantity }}
            <a @click="incrementQuantity(item)" class="button is-small">+</a>
            <a @click="decrementQuantity(item)" class="button is-small">-</a>
        </td>
        <td>{{ getItemTotal(item).toFixed(2) }} €</td>
        <td>
            <button class="delete" @click="removeFromCart(item)">
                
            </button>
        </td>
     </tr>
</template>

<script>
import { ref } from 'vue';
import { useStore } from 'vuex';

export default {
    name: 'CartItem',
    props: {
        initialItem: {
            type: Object,
        },
    },
    setup(props, { emit }) {
        const store = useStore();
        const item = ref(props.initialItem);

        function getItemTotal(item) {
            return item.product.price * item.quantity;
        }

        function incrementQuantity(item) {
            item.quantity += 1;

            updateCart();

        }

        function decrementQuantity(item) {
            if (item.quantity === 0) {
                emit('removeFromCart', item);
            }
            item.quantity -= 1;
        }

        function updateCart() {
            localStorage.setItem('cart', JSON.stringify(store.state.cart));
        }

        function removeFromCart(item) {
            emit('removeFromCart', item);

            updateCart();
        }

        return {
            item,
            getItemTotal,
            incrementQuantity,
            decrementQuantity,
            removeFromCart,
        };

    }

}
</script>