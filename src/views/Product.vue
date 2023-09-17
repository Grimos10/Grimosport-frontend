<template>
    <div class="page-product">
        <div class="columns is-multiline">
            <div class="column is-9">
                <figure class="image mb-6">
                    <img :src="product.get_image" :alt="product.name" />
                </figure>

                <h1 class="title">{{ product.name }}</h1>

                <p>{{ product.description }}</p>
            </div>

            <div class="column is-3">
                <h2 class="subtitle">Informazioni</h2>

                <p><strong>Prezzo: </strong>€{{ product.price }}</p>

                <div class="field has-addons mt-6">
                    <div class="control">
                        <input type="number" class="input" min="1" v-model="quantity" />
                    </div>

                    <div class="control">
                        <a class="button is-dark">Aggiungi al carrello</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios';

export default {
    name: 'Product',
    setup() {
        const route = useRoute();
        const product = ref({});
        const quantity = ref(1);
        const category_slug = route.params.category_slug;
        const product_slug = route.params.product_slug;

        axios.get(`/api/v1/products/${category_slug}/${product_slug}/`).then((response) => {
            product.value = response.data;
        })
        .catch((error) => {
            console.log(error);
        });

        return {
            product,
            quantity,    
        }
    },

}

</script>