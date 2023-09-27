<template>
    <div class="page-my-account">
        <div class="columns is-multiline" v-if="is_staff">
            <div class="column is-12">
                <h1 class="title">Questo è un account staff</h1>
                <button @click="logout()" class="button is-danger">Logout</button>
                <hr/>
                <h2 class="subtitle">Bentornat* {{ username }}, puoi vedere le statistiche di vendita cliccando il bottone qui sotto oppure compila tutti i campi per aggiungere un nuovo prodotto</h2>
            </div>

            <div class="column is-12">
                <router-link to="/stats" class="button is-info">Vai alle statistiche</router-link>
            </div>

            <div class="column is-12 box">

                <div class="columns is-multiline">

                    <form class="column is-6" @submit.prevent="addProduct">
                        
                        <div class="field">
                            <label>Categoria:</label>
                            <div class="control">
                                <select v-model="category">
                                <option v-for="category in categories" :key="category.id" :value="category.name">{{ category.name }}</option>
                            </select>
                            </div>
                        </div>

                        <div class="field">
                            <label>Nome:</label>
                            <div class="control">
                                <input class="input" type="text" v-model="name" required>
                            </div>
                        </div>

                        <div class="field">
                            <label>Slug:</label>
                            <div class="control">
                                <input class="input" type="text" v-model="slug" required>
                            </div>
                        </div>

                        <div class="field">
                            <label>Descrizione:</label>
                            <div class="control">
                                <textarea class="textarea is-small" v-model="description"></textarea>
                            </div>
                        </div>

                        <div class="field">
                            <label>Prezzo:</label>
                            <div class="control">
                                <input class="input" type="number" step="0.01" min="0.01" v-model="price" required>
                            </div>
                        </div>

                        <div class="field">
                            <label>Immagine:</label>
                            <div id="image" class="file has-name">
                                <label class="file-label">
                                    <input class="file-input" type="file" name="resume" @change="handleFileUpload" required>
                                    <span class="file-cta">
                                        <span class="file-icon">
                                            <i class="fas fa-upload"></i>
                                        </span>
                                        <span class="file-label">
                                            Scegli un'immagine
                                        </span>
                                    </span>
                                    <span class="file-name">
                                        <span v-if="image">{{ image.name }}</span>
                                        <span v-else>Nessun file selezionato</span>
                                    </span>
                                </label>
                            </div>
                        </div>

                        <hr>

                        <div class="field">
                            <div class="control">
                                <button class="button is-dark">Aggiungi prodotto</button>
                            </div>
                        </div>

                        
                    </form>
                </div>
            </div>


        </div>
        <div class="columns is-multiline" v-else>
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
        const categories = ref({});
        const category = ref('');
        const name = ref('');
        const description = ref('');
        const slug = ref('');
        const price = ref('');
        const orders = ref([]);
        const username = ref('');
        const is_staff = ref(false);
        const image = ref(null);

        const handleFileUpload = () => {
            image.value = document.querySelector('#image input[type=file]').files[0];

        }

        async function addProduct() {

            if (category.value === 'Summer'){
                category.value = 1;
            }
            else{
                category.value = 2;
            }
            
            const data = new FormData();
            data.append('category', category.value);
            data.append('name', name.value);
            data.append('description', description.value);
            data.append('slug', slug.value);
            data.append('price', price.value);
            data.append('image', image.value);

            console.log(data);

            await axios
            .post('/api/v1/add-product/', data, {
                headers: {
                    'Content-Type': 'multipart/form-data',
                },
            })
            .then((response) => {
                toast({
                    message: 'Prodotto aggiunto con successo.',
                    type: 'is-success',
                    position: 'bottom-right',
                    duration: 3000,
                });
            })
            .catch((error) => {
                console.log(error);

                toast({
                    message: 'Errore durante l\'aggiunta del prodotto.',
                    type: 'is-danger',
                    position: 'bottom-right',
                    duration: 3000,
                });
            });
        }



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

            await axios
            .get('/api/v1/users/me/')
            .then(async (response) => {
                is_staff.value = response.data.is_staff;
                if (is_staff.value) {
                    await axios
                    .get('/api/v1/get-categories/')
                    .then((response) => {
                        categories.value = response.data;
                    })
                    .catch((error) => {
                        console.log(error);
                    });
                }
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
            is_staff,
            categories,
            category,
            name,
            description,
            slug,
            price,
            image,
            handleFileUpload,
            addProduct,
        }
    }
}
</script>