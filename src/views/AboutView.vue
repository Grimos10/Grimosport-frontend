<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title is-1">Info su di noi</h1>
        <p class="subtitle is-3">
          Grimosport nasce con l'idea di soddisfare le esigenze di tutti gli sportivi, un sito dove poter trovare tutto ciò che serve per praticare il proprio sport preferito.
          Il brand nasce dalla passione per lo sport del suo fondatore Biagio Grimolizzi, un giovane imprenditore che ha deciso di investire nel suo sogno.
          Un brand che si è fatto conoscere per la qualità dei suoi prodotti e per la sua affidabilità.
        </p>
      </div>
    </div>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="title is-2">Il nostro team</h2>
      </div>
    </div>

    <div class="columns is-multiline">
      <div class="column is-3" v-for="member in team" :key="member.name">
        <div class="card">
          <div class="card-content">
            <p class="title is-4">{{ member.name }}</p>
            <p class="subtitle is-6">{{ member.role }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  setup() {
    const team = [
      {
        name: 'Biagio Grimolizzi',
        role: 'CEO',
      },
      {
        name: 'Biagio Grimolizzi',
        role: 'CTO',
      },
      {
        name: 'Mario Rossi',
        role: 'Developer',
      },
      {
        name: 'Luigi Bianchi',
        role: 'Developer',
      },
    ];

    return {
      team,
    };
  },
}
</script>
