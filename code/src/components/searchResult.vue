<template>
  <h1 v-if="!hasData">Nenhum dado selecionado</h1>
  <div v-else>
    <b-container class="d-flex flex-column align-items-start justify-content-center">
      <p>Essas são as melhores alternativas de frete que encontramos para você.</p>
        <b-row>
            <b-col cols="7">
                <b-card img-left class="d-flex">
                    <b-card-text>
                        <b class="card-title">Frete com menor valor</b>
                        <p>Transportadora: {{ cheapestTransport.name }}</p>
                        <p>Tempo estimado: {{ cheapestTransport.lead_time }}</p>
                    </b-card-text>
                </b-card>
            </b-col>
            <b-col cols="5">
                <b-card>
                    <b-card-text>
                        <b class="card-title">Preço</b>
                        {{ cheapestTransport.cost }}
                    </b-card-text>
                </b-card>
            </b-col>
        </b-row>
        <br>
        <b-row>
            <b-col cols="7">
                <b-card img-left>
                    <b-card-img src="../assets/clock-regular.svg"/>
                    <b-card-text>
                        <b class="card-title">Frete mais rápido</b>
                        <p>Transportadora: {{ fastestTransport.name }}</p>
                        <p>Tempo estimado: {{ fastestTransport.lead_time }}</p>
                    </b-card-text>
                </b-card>
            </b-col>
            <b-col cols="5">
                <b-card img-src="" img-alt="" img-left class="">
                    <b-card-text>
                        <b class="card-title">Preço</b>
                        {{ fastestTransport.cost }}
                    </b-card-text>
                </b-card>
            </b-col>
        </b-row>
    </b-container>
    <br>
    <div>
        <b-button variant="primary" @click="cleanSearchParameters">Limpar</b-button>
    </div>
  </div>
</template>

<script>
import {
  BContainer,
  BRow,
  BCol,
  BButton,
  BCard,
  BCardText
} from 'bootstrap-vue'

export default {
  components: {
    BContainer,
    BRow,
    BCol,
    BButton,
    BCard,
    BCardText
  },
  props: {
    fastestTransport: {
      type: Object
    },
    cheapestTransport: {
      type: Object
    }
  },
  methods: {
    cleanSearchParameters() {
        this.$emit('cleanSearchParameters')
    }
  },
  computed: {
    hasData() {
      return Object.keys(this.fastestTransport).length > 0 && Object.keys(this.cheapestTransport).length > 0
    }
  }
}
</script>

<style>
.card-title {
    display: block;
}
</style>
