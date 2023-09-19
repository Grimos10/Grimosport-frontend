<template>
    <div class="page-sign-up"> 
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Registrati</h1>

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
                    <div class="field">
                        <label>Conferma password</label>
                        <div class="control">
                            <input class="input" type="password" v-model="passwordConfirm">
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
                            <button class="button is-dark">Registrati</button>
                        </div>
                    </div>

                    <hr>

                    <div class="field">
                        <div class="control">
                            <router-link to="/login" class="button is-text">Hai già un account? Accedi</router-link>
                        </div>
                    </div>

                </form>
            </div>
        </div>

    </div>
</template>

<script>
import { ref } from 'vue';
import axios from 'axios';
import { toast } from 'bulma-toast';
import { useRouter } from 'vue-router';

export default {
    name: 'SignUp',
    setup() {
        const router = useRouter();
        const username = ref('');
        const password = ref('');
        const passwordConfirm = ref('');
        const errors = ref([]);

        function submitForm() {
            errors.value = [];

            if (username.value === '') {
                errors.value.push('Username obbligatorio.');
            }

            if (password.value === '') {
                errors.value.push('Password obbligatoria.');
            }

            if (password.value !== passwordConfirm.value) {
                errors.value.push('Le password non coincidono.');
            }

            if (!errors.value.length) {
                const formData = {
                    username: username.value,
                    password: password.value,
                };

                axios.post('/api/v1/users/', formData).then((response) => {
                    toast({
                        message: 'Registrazione avvenuta con successo, per favore effettua il login.',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 3000,
                        position: 'bottom-right',
                    });

                    router.push('/login');
                }).catch(error => {
                    if (error.response) {
                        for (const property in error.response.data) {
                            errors.value.push(`${property}: ${error.response.data[property]}`);
                        }

                        console.log(JSON.stringify(error.response.data));

                    } else if (error.message) {
                        errors.value.push('Qualcosa è andato storto, per favore riprova più tardi.');

                        console.log(JSON.stringify(error));
                    }
                })
            }



        }

        return {
            username,
            password,
            passwordConfirm,
            errors,
            submitForm,
        }
    }
}
</script>