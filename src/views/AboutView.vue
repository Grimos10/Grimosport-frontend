<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title is-1">Info su di noi</h1>
        <p class="subtitle is-3 has-text-justified">
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

    <hr>
    <br>

    <div class="columns is-multiline is-centered">
      <div class="column is-half">
        <h1 class="title is-1 has-text-danger">INFORMAZIONI IMPORTANTI</h1>
      </div>
    </div>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="subtitle is-2 has-text-danger has-text-justified">
          Questo è un progetto universitario per l'esame di Tecnologie Web. Uno degli aspetti del corso era imparare le basi del web development e creare un sito web,
          quindi il progetto non è perfetto e non è da considerarsi un sito professionale su cui poter comprare realmente vestiti sportivi, è solo un prototipo di
          un possibile sito di e-commerce. Ho deciso, solo per divertimento e imparare nuove cose e non perchè era necessario, di effettuare il deploy del progetto su un server e di implementare 
          un modo per poter simulare pagamenti reali.<br>
          A proposito di ciò ho deciso di utilizzare Stripe, un servizio che permette di effettuare pagamenti online in modo sicuro e veloce e quindi 
          di poter testare il mio sito come se fosse un sito reale, infatti è possibile effettuare pagamenti con una carta di credito fittizia
          (numero: 4242 4242 4242 4242, data di scadenza: 04/24, cvc: 242) e vedere come funziona il processo di acquisto, in modo tale da ritrovarsi poi nel proprio
          account l'ordine appena fatto e quelli precedenti se ne sono stati fatti altri.
        </h2>
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
