<template>
  <div>
    <div class="title">
      <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand class="ml-2">
          <b>{{ appName }}</b>
        </b-navbar-brand>
      </b-navbar>
    </div>
    <search-transport-form :cities="destinyCities" @search="handleSearchParameters"/> 
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
    destinyCities = [],
    searchParameters = []

    return {
      appName, apiData, destinyCities, searchParameters
    }
  },
  watch: {
    apiData(newValue) {
      this.getDestinyCities(newValue)
    }
  },
  created() {
    this.fetchApiData();
    this.appName = 'Melhor Frete'
  },
  methods: {
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
    getDestinyCities(transportList) {
      for ( let transport of transportList) {
        if ( !this.destinyCities.includes(transport.city) ) {
          this.destinyCities.push(transport.city)
        }
      }
    },
    handleSearchParameters (searchParameters) { 
      if (searchParameters) {
        // TODO searchCheapestTransport
        // TODO searchFastestTransport
        console.log(searchParameters)
      } else {
        // TODO abrir modal
        console.log("Falstam infos de busca")
      }
    }
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
