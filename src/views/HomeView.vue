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
      <div
      class="column is-3" id="product"
      v-for="product in latestProducts"
      :key="product.id"
      >
        <div class="box">
          <figure class="image mb-4">
            <img :src="product.get_thumbnail" :alt="product.name" />
          </figure>

          <h3 class="is-size-4">{{ product.name }}</h3>
          <p class="is-size-6 has-text-grey">€{{ product.price }}</p>

          <router-link :to="product.get_absolute_url" class="button is-dark mt-4"> Vedi dettagli </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { ref } from 'vue';
import { onMounted } from 'vue';

export default {
  name: 'HomeView',
  setup() {
    const latestProducts = ref([]);

    axios.get('/api/v1/latest-products/').then((response) => {
      latestProducts.value = response.data;
    })
    .catch((error) => {
      console.log(error);
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
