<template>
    <div class="page-sign-up"> 
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Accedi</h1>

                <form @submit.prevent="submitForm">

                    <div class="field">
                          <label>Username</label>
                        <div class="control">
                            <input class="input" type="text" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label>Password</label>
                        <div class="control">
                            <input class="input" type="password" v-model="password">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p>Si sono verificati i seguenti errori:</p>
                        <ul>
                            <li v-for="error in errors" :key="error">{{ error }}</li>
                        </ul>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-dark">Accedi</button>
                        </div>
                    </div>

                    <hr>

                    <div class="field">
                        <div class="control">
                            <router-link to="/signup" class="button is-text">Non hai un account? Registrati</router-link>
                        </div>
                    </div>


                </form>
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
    name: 'LogIn',
    setup() {
        const username = ref('');
        const password = ref('');
        const errors = ref([]);
        const store = useStore();
        const route = useRoute();
        const is_staff = ref(false);

        const submitForm = async () => {
            axios.defaults.headers.common['Authorization'] = '';
            localStorage.removeItem('token');

            errors.value = [];

            if (!username.value) {
                errors.value.push('Inserisci il tuo username.');
            }

            if (!password.value) {
                errors.value.push('Inserisci la tua password.');
            }

            if (errors.value.length) {
                return;
            }

            await axios
                .post('/api/v1/token/login/', {
                    username: username.value,
                    password: password.value,
                })
                .then(async (response) => {
                    const token = response.data.auth_token;
                    store.commit('setToken', token);
                    axios.defaults.headers.common['Authorization'] = `Token ${token}`;
                    localStorage.setItem('token', token);

                    store.commit('setUsername', username.value);
                    localStorage.setItem('username', username.value);

                    toast({
                        message: 'Login effettuato con successo.',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 3000,
                        position: 'bottom-right',
                    });

                    await axios.get('/api/v1/users/me/').then((response) => {
                        is_staff.value = response.data.is_staff;
                    })
                    .catch((error) => {
                        console.log(error);
                    });

                    if (is_staff.value) {
                        router.push('/myaccount');
                    }
                    else {
                        const toPath = route.query.to || '/cart';
                        router.push(toPath);
                    }
                    
                })
                .catch((error) => {
                    if (error.response) {
                        for (const property in error.response.data) {
                            errors.value.push(`${property}: ${error.response.data[property]}`);
                        }
                    } else {
                        errors.value.push('Errore durante la richiesta di login.');

                        console.log(JSON.stringify(error));
                    }
                });
        };

        return {
            username,
            password,
            errors,
            submitForm,
        };

    }
}
</script>