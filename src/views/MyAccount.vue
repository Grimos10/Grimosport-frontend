<template>
    <div class="page-my-account">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Il mio account</h1>
            </div>
<!--
            <div class="column is-12 box">
                <h2 class="subtitle">Informazioni account</h2>

                <div class="columns">
                    <div class="column is-6">
                        <strong>Username:</strong> {{ store.state.username }}
                    </div>
                    <div class="column is-6">
                        <strong>Email:</strong> {{ store.state.email }}
                    </div>
                </div>
            </div>
-->

            <div class="column is-12">
                <button @click="logout()" class="button is-danger">Logout</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { toast } from 'bulma-toast';
import { ref } from 'vue';
import { useStore } from 'vuex';
import { useRoute } from 'vue-router';
import router from '@/router';

export default {
    name: 'MyAccount',
    setup() {
        const store = useStore();


        function logout() {
            axios.defaults.headers.common['Authorization'] = '';

            localStorage.removeItem('token');
            localStorage.removeItem('username');
            localStorage.removeItem('userid');

            store.commit('removeToken');

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
        }
    }
}
</script>