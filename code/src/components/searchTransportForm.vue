<template>
  <div>
    <form @submit.prevent="handleSearch">
      <b>Insira o destino e o peso</b>
      <input label="Destino" v-model="destino">  <!-- TODO tem que receber as cidades disponíveis e fazer um dropdown -->
      <input label="Peso" v-model="peso">
      <button>Analisar</button>
    </form>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        destino: '',
        peso:''
      }
    },
    methods: {
      handleSearch() {
        // TODO função handleSubmit:
        // Se tiver destino E peso passar o resultado pro componente de resultado
        // em caso contrário, abrir um modal e zerar os valores de destino e peso
        console.log(this.destino, this.peso)
      }
    }
  }
</script>

<style scoped>
  input {
    margin: 10px;
    display: block;
  }
</style>