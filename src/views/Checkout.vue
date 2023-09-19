<template>
    <div class="page-checkout">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Checkout</h1>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Riepilogo ordine</h2>

                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Prodotto</th>
                            <th>Prezzo</th>
                            <th>Quantità</th>
                            <th>Totale</th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr v-for="item in cart.items" :key="item.product.id">
                            <td>{{ item.product.name }}</td>
                            <td>{{ item.product.price }}€</td>
                            <td>{{ item.quantity }}</td>
                            <td>{{ getItemTotal(item).toFixed(2) }}€</td>
                        </tr>
                    </tbody>

                    <tfoot>
                        <tr>
                            <td colspan="2">Totale</td>
                            <td>{{ cartTotalLenght }}</td>
                            <td>{{ cartTotalPrice.toFixed(2) }}€</td>
                        </tr>
                    </tfoot>
                </table>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Informazioni di spedizione</h2>

                <p class="has-text-grey mb-4">* Tutti i campi sono richiesti</p>

                <div class="columns is-multiline">
                    <div class="column is-6">
                        <div class="field">
                            <label>Nome*</label>
                            <div class="control">
                                <input class="input" type="text" v-model="first_name">
                            </div>
                        </div>

                        <div class="field">
                            <label>Cognome*</label>
                            <div class="control">
                                <input class="input" type="text" v-model="last_name">
                            </div>
                        </div>

                        <div class="field">
                            <label>Email*</label>
                            <div class="control">
                                <input class="input" type="email" v-model="email">
                            </div>
                        </div>

                        <div class="field">
                            <label>Telefono*</label>
                            <div class="control">
                                <input class="input" type="text" v-model="phone">
                            </div>
                        </div>
                    </div>

                    <div class="column is-6">
                        <div class="field">
                            <label>Indirizzo*</label>
                            <div class="control">
                                <input class="input" type="text" v-model="address">
                            </div>
                        </div>
                        <div class="field">
                            <label>Città*</label>
                            <div class="control">
                                <input class="input" type="text" v-model="place">
                            </div>
                        </div>
                        <div class="field">
                            <label>CAP*</label>
                            <div class="control">
                                <input class="input" type="text" v-model="zipcode">
                            </div>
                        </div>
                    </div>

                </div>

                <div class="notification is-danger mt-4" v-if="errors.length">
                        <p>Si sono verificati i seguenti errori:</p>
                        <ul>
                            <li v-for="error in errors" :key="error">{{ error }}</li>
                        </ul>
                </div>

                <hr>

                <div id="card-element" class="mb-5"></div>

                <template v-if="cartTotalLenght">
                    <hr>

                    <button class="button is-dark" @click="submitForm">Paga</button>
                </template>

                <template v-else>
                    <div class="has-text-centered">
                        <p>Il tuo carrello è vuoto.</p>
                    </div>
                </template>

            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { useStore } from 'vuex';
import { ref, computed, onMounted } from 'vue';

export default {
    name: 'Checkout',
    setup() {
        const store = useStore();
        const cart = ref({ items: [] });
        const stripe = ref({});
        const card = ref({});
        const first_name = ref('');
        const last_name = ref('');
        const email = ref('');
        const address = ref('');
        const phone = ref('');
        const zipcode = ref('');
        const place = ref('');
        const errors = ref([]);

        onMounted(async () => {
            cart.value = store.state.cart;
        });

        function getItemTotal(item) {
            return item.product.price * item.quantity;
        }

        function submitForm() {
            errors.value = [];

            if (!first_name.value) {
                errors.value.push('Inserisci il tuo nome.');
            }

            if (!last_name.value) {
                errors.value.push('Inserisci il tuo cognome.');
            }

            if (!email.value) {
                errors.value.push('Inserisci la tua email.');
            }

            if (!address.value) {
                errors.value.push('Inserisci il tuo indirizzo.');
            }

            if (!phone.value) {
                errors.value.push('Inserisci il tuo numero di telefono.');
            }

            if (!zipcode.value) {
                errors.value.push('Inserisci il tuo CAP.');
            }

            if (!place.value) {
                errors.value.push('Inserisci la tua città.');
            }

            if (errors.value.length) {
                return;
            }
            
        }

        const cartTotalLenght = computed(() => {
            return cart.value.items.reduce((acc, curVal) => {
                return acc += curVal.quantity;
            }, 0);
        });

        const cartTotalPrice = computed(() => {
            return cart.value.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity;
            }, 0);
        });

        return {
            cart,
            stripe,
            card,
            first_name,
            last_name,
            email,
            address,
            phone,
            zipcode,
            place,
            errors,
            getItemTotal,
            cartTotalLenght,
            cartTotalPrice,
            submitForm,
        };
    },
    
}
</script>