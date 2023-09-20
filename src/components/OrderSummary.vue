<template>
    <div class="box mb-4">
        <h3 class="is-size-4 mb-4">Riepilogo ordine</h3>

        <h4 class="is-size-5 mb-4">Prodotti</h4>

        <table class="table is-fullwidth">
            <thead>
                <tr>
                    <th>Prodotto</th>
                    <th>Prezzo</th>
                    <th>Quantità</th>
                    <th>Totale</th>
                </tr>
            </thead>

            <tbody>
                <tr v-for="item in order.items" :key="item.product.id">
                    <td>{{ item.product.name }}</td>
                    <td>{{ item.product.price }}€</td>
                    <td>{{ item.quantity }}</td>
                    <td>{{ getItemTotal(item).toFixed(2) }}€</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import { computed } from 'vue';

export default {
    name: 'OrderSummary',
    props: {
        order: {
            type: Object,
        },
    },
    setup(props) {

        const getItemTotal = (item) => {
            return item.product.price * item.quantity;
        };

        const orderTotalLengh = (order) => {
            return order.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0);
        };

        return {
            getItemTotal,
            orderTotalLengh,
        };
    },
};
</script>