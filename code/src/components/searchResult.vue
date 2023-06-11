<template>
  <h1 v-if="!hasData">Nenhum dado selecionado</h1>
  <div v-else>
    {{ fastestTransport }}
    {{ cheapestTransport }}
  </div>
</template>

<script>

export default {
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