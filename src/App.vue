<template>
  <div id="wrapper">
    <nav class="navbar is-dark">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item"><strong>NOMEAPP</strong></router-link>

        <a class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbar-menu" @click="showMobileMenu = !showMobileMenu">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu" id="navbar-menu" v-bind:class="{'is-active': showMobileMenu }">
        <div class="navbar-end">
          <router-link to="/summer" class="navbar-item">Summer</router-link>
          <router-link to="/winter" class="navbar-item">Winter</router-link>

          <div class="navbar-item">
            <div class="buttons">
              <router-link to="/log-in" class="button is-light">Log in</router-link>

              <router-link to="/cart" class="button is-success">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span>Cart ({{ cartTotalLength }})</span>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <section class="section">
      <router-view/>
    </section>

<!-- possible footer section
  <footer class="footer">
  </footer>
-->

  </div>
</template>

<script>
import { ref, onBeforeMount, computed, onMounted } from 'vue'
import { useStore } from 'vuex'

export default {
  setup() {
    const showMobileMenu = ref(false)
    const cart = ref({items: []})
    const store = useStore()


    onBeforeMount(() => {
      store.commit('initializeStore')
      cart.value = store.state.cart
    })

    const cartTotalLength = computed(() => {
      let totalLenght = 0
      cart.value.items.forEach(item => {
        totalLenght += item.quantity
      })
      return totalLenght
    })


    return {
      showMobileMenu,
      cart,
      cartTotalLength
    }
  },
}
</script>

<style lang="scss">
@import '../node_modules/bulma/';
</style>
