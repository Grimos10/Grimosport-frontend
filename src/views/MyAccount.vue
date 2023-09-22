<template>
    <div class="page-my-account">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Il mio account</h1>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Informazioni account</h2>

                <div class="columns">
                    <div class="column is-6">
                        <div>Ciao {{ username }} questi sono i tuoi ordini</div> 
                    </div>
                </div>
            </div>


            <div class="column is-12">
                <button @click="logout()" class="button is-danger">Logout</button>
            </div>

            <hr>

            <div class="column is-12">
                <h2 class="subtitle">I miei ordini</h2>

                <OrderSummary
                    v-for="order in orders"
                    :key="order.id"
                    :order="order" />
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { toast } from 'bulma-toast';
import { ref, onMounted } from 'vue';
import { useStore } from 'vuex';
import { useRoute } from 'vue-router';
import router from '@/router';

import OrderSummary from '@/components/OrderSummary.vue';

export default {
    name: 'MyAccount',
    components: {
        OrderSummary,
    }, 
    setup() {
        const store = useStore();
        const orders = ref([]);
        const username = ref('');

        onMounted(async () => {
            store.commit('setIsLoading', true);

            await axios
            .get('/api/v1/orders/')
            .then((response) => {
                orders.value = response.data;
            })
            .catch((error) => {
                console.log(error);
            });

            username.value = store.state.username;
            username.value = localStorage.getItem('username');

            store.commit('setIsLoading', false);
        });


        function logout() {
            axios.defaults.headers.common['Authorization'] = '';

            localStorage.removeItem('token');
            localStorage.removeItem('username');
            localStorage.removeItem('userid');

            store.commit('removeToken');
            store.commit('removeUsername');

            toast({
                message: 'Logout effettuato con successo.',
                type: 'is-success',
                position: 'bottom-right',
                duration: 3000,
            });

            router.push('/');
        }

        return {
            logout,
            orders,
            username,
        }
    }
}
</script>