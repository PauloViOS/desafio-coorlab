<template>
    <b-container>
        <div>
            <div class="title">
                <b-navbar toggleable="lg" type="dark" variant="info">
                    <b-navbar-brand class="ml-2">
                        <b>{{ appName }}</b>
                    </b-navbar-brand>
                </b-navbar>
            </div>
            <b-container>
                <b-row>
                    <b-col>
                        <search-transport-form :cities="destinationCities" @search="handleSearchParameters"/>
                    </b-col>
                    <b-col>
                        <search-result :fastestTransport="fastestTransport" :cheapestTransport="cheapestTransport"/>
                    </b-col>
                </b-row>
            </b-container>
        </div>
    </b-container>
</template>

<script>
import {
  BNavbar,
  BNavbarBrand,
  BContainer,
  BRow,
  BCol
} from 'bootstrap-vue'

import searchTransportForm from './searchTransportForm.vue'
import searchResult from './searchResult.vue'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
    BContainer,
    BRow,
    BCol,
    searchTransportForm,
    searchResult
  },
  data() {
    const appName = '',
    apiData = null,
    destinationCities = [],
    searchParameters = [],
    fastestTransport = {},
    cheapestTransport = {}

    return {
      appName, apiData, destinationCities, searchParameters, fastestTransport, cheapestTransport
    }
  },
  watch: {
    apiData(newValue) {
      this.getDestinationCities(newValue)
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
    getDestinationCities(transportList) {
      for (let transport of transportList) {
        if ( !this.destinationCities.includes(transport.city) ) {
          this.destinationCities.push(transport.city)
        }
      }
    },
    handleSearchParameters (searchParameters) { 
      if (searchParameters.length === 2) { // TODO é a melhor forma de fazer isso?
        this.cheapestTransport = this.searchCheapestTransport(searchParameters)
        this.fastestTransport = this.searchFastestTransport(searchParameters)
      } else {
        // TODO abrir modal
        console.log("Faltam infos de busca")
      }
    },
    searchFastestTransport(parameters) {
      let destination = parameters[0]
      let costCategory = this.getCostCategory(parameters[1])
      let fastest = {
        'name': '',
        'lead_time': '999h',
        'cost': ''
      }

      for (let transport of this.apiData) {
        let leadTimeTransport = parseInt(transport.lead_time.slice(0,-1))
        let leadTimeFastest = parseInt(fastest.lead_time.slice(0,-1))
        if (transport.city === destination && leadTimeTransport < leadTimeFastest) {
          fastest.name = transport.name
          fastest.lead_time = transport.lead_time
          fastest.cost = transport[costCategory]
        }
      }

      return fastest
    },
    searchCheapestTransport(parameters) {
      let destination = parameters[0]
      let costCategory = this.getCostCategory(parameters[1])
      let cheapest = {
        'name': '',
        'lead_time': '',
        'cost': 'R$ 9999'
      }

      for (let transport of this.apiData) {
        let costTransport = parseFloat(transport[costCategory].slice(3))
        let costCheapest = parseFloat(cheapest.cost.slice(3))
        if (transport.city === destination && costTransport < costCheapest) {
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
