<template>
  <div>
    <b-form @submit.prevent="handleSearch">
      <b>Insira o destino e o peso</b>
      <label>
        <b>Destino</b>
        <b-form-select v-model="transportDestiny" :options="cities">
            <b-form-select-option value="" hidden selected>Selecione o destino</b-form-select-option>
        </b-form-select>
      </label>
      <label>
        <b>Peso</b> <!--TODO deveria aceitar vírgula?-->
        <b-form-input type="number" v-model="transportWeight" placeholder="Peso da carga em kg"></b-form-input>
      </label>
      <b-button type="submit" variant="primary">Analisar</b-button> <!-- TODO mudar cor / colocar replace de . por , no tratamento do input-->
    </b-form>
  </div>
</template>

<script>
import {
  BForm,
  BFormSelect,
  BButton
} from 'bootstrap-vue'

export default {
  components: {
    BForm,
    BFormSelect,
    BButton
  },
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