<template>
    <div class="stats-page">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Statistiche</h1>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Statistiche di vendita</h2>

                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Prodotto</th>
                            <th>Quantità totale di articoli di questo prodotto acquistati</th>
                            <th>Quante volte il prodotto è stato in un ordine</th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr v-for="item in stats" :key="item.id_product">
                            <td>{{ item.name_product }}</td>
                            <td v-if="item.quantity_orders_product">{{ item.quantity_orders_product }}</td>
                            <td v-else>0</td>
                            <td>{{ item.quantity_orders }}</td>
                        </tr>
                    </tbody>

                </table>
            </div>
        </div>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useStore } from 'vuex';
import axios from 'axios';

export default {
    name: 'Stats',

    setup() {
        const store = useStore();
        const stats = ref([]);

        onMounted(async () => {
            store.commit('setIsLoading', true);

            await axios
            .get('/api/v1/stats/')
            .then((response) => {
                stats.value = response.data;
            })
            .catch((error) => {
                console.log(error);
            });

            store.commit('setIsLoading', false);

        });

        return {
            stats,
        };
    },
};
</script>