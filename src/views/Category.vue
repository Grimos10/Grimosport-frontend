<template>
    <div class="page-category">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
            </div>

            <ProductBox v-for="product in category.products" :key="product.id" :product="product"></ProductBox>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { ref, onMounted, watch } from 'vue';
import { useRoute } from 'vue-router';
import { useStore } from 'vuex';
import { toast } from 'bulma-toast';

import ProductBox from '@/components/ProductBox.vue';

export default {
    name: 'Category',
    components: {
        ProductBox,
    },
    setup() {
        const category = ref({ products: []});
        const route = useRoute();
        const category_slug = route.params.category_slug;
        const store = useStore();

        store.commit('setIsLoading', true);

        onMounted(async () => {
            await axios.get(`/api/v1/products/${category_slug}/`).then((response) => {
                category.value = response.data;
            })
            .catch((error) => {
                console.log(error);

                toast({
                    message: 'Errore nel caricamento dei prodotti',
                    type: 'is-danger',
                    dismissible: true,
                    pauseOnHover: true,
                    duration: 2000,
                    position: 'bottom-right',
                });
            });

            store.commit('setIsLoading', false);
        });

        watch(() => route.params.category_slug, async (category_slug) => {
            store.commit('setIsLoading', true);

            await axios.get(`/api/v1/products/${category_slug}/`).then((response) => {
                category.value = response.data;
            })
            .catch((error) => {
                console.log(error);

                toast({
                    message: 'Errore nel caricamento dei prodotti',
                    type: 'is-danger',
                    dismissible: true,
                    pauseOnHover: true,
                    duration: 2000,
                    position: 'bottom-right',
                });
            });

            store.commit('setIsLoading', false);
        });

        return {
            category,
        };
    },
}

</script>