<template>
    <div class="d-flex flex-column">
        <div class="title">
            <b-navbar toggleable="lg" type="light" variant="info">
                <b-navbar-brand class="ml-2">
                    <b>{{ appName }}</b>
                </b-navbar-brand>
            </b-navbar>
        </div>
        <div>
            <b-row>
                <b-col cols="6">
                    <search-transport-form
                        :cities="destinationCities"
                        ref="searchTransportForm"
                        @search="handleSearchParameters"
                    />
                </b-col>
                <b-col cols="6">
                    <search-result
                        :fastestTransport="fastestTransport"
                        :cheapestTransport="cheapestTransport"
                        @cleanSearchParameters="cleanSearchParameters"
                    />
                </b-col>
            </b-row>
        </div>
        <no-search-parameters-modal ref="noSearchParametersModal" />
    </div>
</template>

<script>
import {
  BNavbar,
  BNavbarBrand,
  BRow,
  BCol
} from 'bootstrap-vue'

import searchTransportForm from './searchTransportForm.vue'
import searchResult from './searchResult.vue'
import noSearchParametersModal from './noSearchParametersModal.vue'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
    BRow,
    BCol,
    searchTransportForm,
    searchResult,
    noSearchParametersModal
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
        this.$refs.noSearchParametersModal.openModal()
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
    },
    cleanSearchParameters() {
        this.fastestTransport = {}
        this.cheapestTransport = {}
        this.$refs["searchTransportForm"].clearSearchFields()
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
