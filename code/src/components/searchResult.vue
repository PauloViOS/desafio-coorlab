<template>
  <h1 v-if="!hasData">Nenhum dado selecionado</h1>
  <div v-else>
    Essas são as melhores alternativas de frete que encontramos para você.
    <b-container>
        <b-row>
            <b-col>
                <b-card img-src="" img-alt="" img-left class="">
                    <b-card-text>
                        <b>Frete com menor valor</b>
                        {{ cheapestTransport.name }}
                        {{ cheapestTransport.lead_time }}
                    </b-card-text>
                </b-card>
            </b-col>
            <b-col>
                <b-card img-src="" img-alt="" img-left class="">
                    <b-card-text>
                        <b>Preço</b>
                        {{ cheapestTransport.cost }}
                    </b-card-text>
                </b-card>
            </b-col>
        </b-row>
        <b-row>
            <b-col>
                <b-card img-src="" img-alt="" img-left class="">
                    <b-card-text>
                        <b>Frete mais rápido</b>
                        {{ fastestTransport.name }}
                        {{ fastestTransport.lead_time }}
                    </b-card-text>
                </b-card>
            </b-col>
            <b-col>
                <b-card img-src="" img-alt="" img-left class="">
                    <b-card-text>
                        <b>Preço</b>
                        {{ fastestTransport.cost }}
                    </b-card-text>
                </b-card>
            </b-col>
        </b-row>
    </b-container>
    <b-button @click="cleanSearchParameters">Limpar</b-button> <!--TODO função pra limpar os valores de fastest e cheapest-->
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
    
  },
  computed: {
    hasData() {
      return Object.keys(this.fastestTransport).length > 0 && Object.keys(this.cheapestTransport).length > 0
    }
  }
}
</script>
