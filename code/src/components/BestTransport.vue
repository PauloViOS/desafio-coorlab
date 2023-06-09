<template>
  <div>
    <div class="title">
      <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand class="ml-2">
          <b>{{ appName }}</b>
        </b-navbar-brand>
      </b-navbar>
    </div>
    <search-transport-form :cities="destinyCities"/> <!-- TODO passar as ciddades de destino como prop  -->
  </div>
</template>

<script>
import {
  BNavbar,
  BNavbarBrand,
} from 'bootstrap-vue'

import searchTransportForm from './searchTransportForm.vue'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
    searchTransportForm
  },
  data() {
    const appName = '',
    apiData = null,
    destinyCities = []

    return {
      appName, apiData, destinyCities
    }
  },
  watch: {
    apiData(newValue) {
      this.getDestinyCities(newValue)
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina

    this.fetchApiData();
    this.appName = 'Melhor Frete'
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    fetchApiData() {
      fetch('http://localhost:3000/transport')
        .then(response => response.json())
        .then(data => {
          this.apiData = data;
        })
        .catch(error => {
          console.log(error);
        })
    },
    // TODO função pra pegar as cidades disponíveis como destino
    getDestinyCities(transportList) {
      for ( let transport of transportList) {
        if ( !this.destinyCities.includes(transport.city) ) {
          this.destinyCities.push(transport.city)
        }
      }
    },
    methodFoo() {
    console.log(this.appName)
    },
  }
}
</script>

<style scoped>
.title .navbar {
  background-color: #00aca6 !important;
}

.title .navbar-brand {
  margin-left: 20px;
}

</style>
