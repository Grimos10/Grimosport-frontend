<template>
    <div class="page-search">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Cerca</h1>

                <h2 class="is-size-5 has-text-grey">Termine di ricerca: "{{ query }}"</h2>
                <h2 class="is-size-5 has-text-grey">Risultati: {{ products.length }}</h2>

            </div>

            <ProductBox v-for="product in products" :key="product.id" :product="product" />
        </div>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';

import axios from 'axios';
import ProductBox from '@/components/ProductBox.vue';
import { useStore } from 'vuex';

export default {
    name: 'Search',
    components: {
        ProductBox,
    },
    setup() {

        const products = ref([]);
        const query = ref('');
        const store = useStore();


        onMounted(async () => {
            store.commit('setIsLoading', true);
            const params = new URLSearchParams(window.location.search.substring(1));
            if (!params.has('query')) {
                return;
            }
            query.value = params.get('query');

            await axios
            .post('/api/v1/products/search/', {
                query: query.value,
            })
            .then((response) => {
                products.value = response.data;
            })
            .catch((error) => {
                console.log(error);
            });

            store.commit('setIsLoading', false);
        });


        return {
            products,
            query,
        }
    },
}
</script>
