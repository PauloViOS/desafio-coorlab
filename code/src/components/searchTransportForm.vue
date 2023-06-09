<template>
  <div>
    <form @submit.prevent="handleSearch">
      <b>Insira o destino e o peso</b>
      <label>
        <b>Destino</b>
        <select v-model="transportDestiny">
          <option v-for="(city, idx) in cities" :key="idx">
            {{ city }}
          </option>
        </select>
      </label>
      <label>
        <b>Peso</b>
        <input v-model="transportWeight">
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
        if (this.transportDestiny && this.transportWeight) {
          console.log(this.transportDestiny, this.transportWeight) // TODO passar infos de volta pro pai
        } else {
          console.log("Falta info") // TODO função para abrir modal
        }
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