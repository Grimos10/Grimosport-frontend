<template>
    <div class="page-success">
        <div class="columns is-multiline"> 
            <div class="column is-12">
                <h1 class="title is-1">Grazie per il tuo ordine!</h1>

                <p class="subtitle is-3">Il tuo ordine è stato registrato con successo e sarà spedito entro 48 ore</p>
            </div>
        </div>
    </div>
</template>

<script>
import { useStore } from 'vuex';

export default {
    name: 'Success',
    setup() {
        const store = useStore();

        store.commit('initializeStore')
        return {
            
        }
    }
}

</script>