<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
      <div class="hero-body has-text-centered">
        <h1 class="title mb-6">
          Home
        </h1>
        <h2 class="subtitle">
          Welcome to the home page
        </h2>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Ultimi prodotti usciti</h2>
      </div> 

      <ProductBox v-for="product in latestProducts" :key="product.id" :product="product"></ProductBox>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { ref } from 'vue';
import { onMounted } from 'vue';
import { useStore } from 'vuex';

import ProductBox from '@/components/ProductBox.vue';

export default {
  name: 'HomeView',
  components: {
    ProductBox,
  },
  setup() {
    const latestProducts = ref([]);
    const store = useStore();

    onMounted(async () => {
      store.commit('setIsLoading', true);

      await axios.get('/api/v1/latest-products/').then((response) => {
        latestProducts.value = response.data;
      })
      .catch((error) => {
        console.log(error);
      });

      store.commit('setIsLoading', false);
    });

    return {
      latestProducts,
    };
  },
  
}
</script>

<style scoped>
  .image {
    margin-top: -1.25rem;
    margin-left: -1.25rem;
    margin-right: -1.25rem;
    /*height: 237px;*/
  }

  img {
    height: 237px;
  }

  #product {
    cursor: pointer;
    /*height: 100%;
    max-height: 312px !important;*/
  }

</style>
