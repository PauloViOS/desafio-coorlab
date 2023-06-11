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
    <search-result :fastestTransport="fastestTransport" :cheapestTransport="cheapestTransport"/>
  </div>
</template>

<script>
import {
  BNavbar,
  BNavbarBrand,
} from 'bootstrap-vue'

import searchTransportForm from './searchTransportForm.vue'
import searchResult from './searchResult.vue'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
    searchTransportForm,
    searchResult
  },
  data() {
    const appName = '',
    apiData = null,
    destinyCities = [],
    searchParameters = [],
    fastestTransport = {},
    cheapestTransport = {}

    return {
      appName, apiData, destinyCities, searchParameters, fastestTransport, cheapestTransport
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
      for (let transport of transportList) {
        if ( !this.destinyCities.includes(transport.city) ) {
          this.destinyCities.push(transport.city)
        }
      }
    },
    handleSearchParameters (searchParameters) { 
      if (searchParameters) {
        this.cheapestTransport = this.searchCheapestTransport(searchParameters)
        this.fastestTransport = this.searchFastestTransport(searchParameters)
      } else {
        // TODO abrir modal
        console.log("Falstam infos de busca")
      }
    },
    searchFastestTransport(parameters) {
      let destiny = parameters[0]
      let costCategory = this.getCostCategory(parameters[1])
      let fastest = {
        'name': '',
        'lead_time': '999h',
        'cost': ''
      }

      for (let transport of this.apiData) {
        let leadTimeTransport = parseInt(transport.lead_time.slice(0,-1))
        let leadTimeFastest = parseInt(fastest.lead_time.slice(0,-1))
        if (transport.city === destiny && leadTimeTransport < leadTimeFastest) {
          fastest.name = transport.name
          fastest.lead_time = transport.lead_time
          fastest.cost = transport[costCategory]
        }
      }

      return fastest
    },
    searchCheapestTransport(parameters) {
      let destiny = parameters[0]
      let costCategory = this.getCostCategory(parameters[1])
      let cheapest = {
        'name': '',
        'lead_time': '',
        'cost': 'R$ 9999'
      }

      for (let transport of this.apiData) {
        let costTransport = parseFloat(transport[costCategory].slice(3))
        let costCheapest = parseFloat(cheapest.cost.slice(3))
        if (transport.city === destiny && costTransport < costCheapest) {
          cheapest.name = transport.name
          cheapest.lead_time = transport.lead_time
          cheapest.cost = transport[costCategory]
        }
      }

      return cheapest
    },
    getCostCategory(weight) {
      let costCategory = ''
      if (weight <= 100) {
        costCategory = 'cost_transport_light'
      } else {
        costCategory = 'cost_transport_heavy'
      }
      return costCategory
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
