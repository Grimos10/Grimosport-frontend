<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
      <div class="hero-body has-text-centered">
        <h1 class="title mb-6">
          GRIMOSPORT
        </h1>
        <h2 class="subtitle">
          Benvenuto nella home page di grimosport dove puoi trovare i nostri prodotti più recenti 
        </h2>
      </div>
    </section>

    <div v-if="(!$store.state.isAuthenticated) || (isEmpty)" class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Ultimi prodotti usciti</h2>
      </div> 

      <ProductBox v-for="product in latestProducts" :key="product.id" :product="product"></ProductBox>
      
    </div>

    <div v-else-if="$store.state.isAuthenticated" class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Ciao {{ username }} ecco per te cose simili a prodotti che hai acquistato</h2>
      </div> 

      <ProductBox v-for="product in moreBuyedProducts" :key="product.id" :product="product"></ProductBox>
      <hr>
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
    const moreBuyedProducts = ref([]);
    const isEmpty = ref(false);
    const username = ref('');
    const store = useStore();

    onMounted(async () => {
      store.commit('setIsLoading', true);

      await axios.get('/api/v1/latest-products/').then((response) => {
        latestProducts.value = response.data;
      })
      .catch((error) => {
        console.log(error);
      });

      if (store.state.isAuthenticated){
        username.value = localStorage.getItem('username');
        await axios.get('/api/v1/more-buyed-products/').then((response) => {
        moreBuyedProducts.value = response.data;
        if (moreBuyedProducts.value.length === 0) {
          isEmpty.value = true;
        }
        })
        .catch((error) => {
          console.log(error);
        });
      }

      store.commit('setIsLoading', false);
    });

    return {
      latestProducts,
      moreBuyedProducts,
      isEmpty,
      username,
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
