<template>
    <div class="d-flex flex-column align-items-center ">
        <b>Insira o destino e o peso</b>
        <b-form @submit.prevent="handleSearch">
            <b-form-group
                id="destination-group"
                label="Destino"
                label-for="destination-input"
            >
                <b-form-select v-model="transportDestination" :options="cities"> <!-- TODO ainda não tá estilizado-->
                    <b-form-select-option value="" hidden selected>Selecione o destino</b-form-select-option>
                </b-form-select>
            </b-form-group>

            <b-form-group
                id="weight-group"
                label="Peso"
                label-for="weight-input"
            >
                <b-form-input type="number" v-model="transportWeight" placeholder="Peso da carga em kg"></b-form-input>
            </b-form-group>
        
            <b-button type="submit" variant="primary">Analisar</b-button> <!-- TODO mudar cor / input não está aceitando . e ,-->
        </b-form>
    </div>
</template>

<script>
import {
  BForm,
  BFormGroup,
  BFormSelect,
  BFormSelectOption,
  BFormInput,
  BButton
} from 'bootstrap-vue'

export default {
  components: {
    BForm,
    BFormGroup,
    BFormSelect,
    BFormSelectOption,
    BFormInput,
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
      transportDestination: '',
      transportWeight: ''
    }
  },
  methods: {
    handleSearch() {
      let searchParameters = []
      if (this.transportDestination && this.transportWeight) {
        searchParameters.push(this.transportDestination, this.transportWeight)  
      }
      this.$emit('search', searchParameters)
    },
    clearSearchFields() {
        this.transportDestination = ''
        this.transportWeight = ''
    }
  }
}
</script>

<style scoped>
  /* TODO Refazer os estilos de maneira mais organizada */

</style>