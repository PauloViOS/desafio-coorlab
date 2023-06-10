<template>
  <div>
    <form @submit.prevent="handleSearch">
      <b>Insira o destino e o peso</b>
      <label>
        <b>Destino</b>
        <select v-model="transportDestiny">
          <option value="" hidden>Selecione o destino</option>
          <option v-for="(city, idx) in cities" :key="idx">
            {{ city }}
          </option>
        </select>
      </label>
      <label>
        <b>Peso</b> <!--TODO deveria aceitar vírgula?-->
        <input type="number" v-model="transportWeight" placeholder="Peso da carga em kg">
      </label>
      <button>Analisar</button>
    </form>
  </div>
</template>

<script>
  export default {
    props: {
      cities: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        transportDestiny: '',
        transportWeight:''
      }
    },
    methods: {
      handleSearch() {
        let searchParameters = []
        if (this.transportDestiny && this.transportWeight) {
          searchParameters.push(this.transportDestiny, this.transportWeight)  
        }
        this.$emit('search', searchParameters)
      }
    }
  }
</script>

<style scoped>
  /* TODO Refazer os estilos de maneira mais organizada */
  input {
    margin: 10px;
    display: block;
  }

  label {
    margin: 10px;
    display: block;
  }

  b {
    display: block;
  }
</style>