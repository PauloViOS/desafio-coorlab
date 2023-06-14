<template>
    <b-modal centered hide-header rounded ref="modal">
        <div class="d-flex align-items-center justify-content-center">
            <img src="../assets/exclamation-solid.svg" class="exclamation">
        </div>
        <div class="d-flex align-items-center modal-body">
            <h2>Insira os valores para realizar a análise.</h2>
        </div>
        <template #modal-footer="{ok}">
            <b-button variant="info" @click="ok()">
                Fechar
            </b-button>
        </template>
    </b-modal>
</template>

<script>
import {
  BModal,
  BButton
} from 'bootstrap-vue'

export default {
  components: {
    BModal,
    BButton
  },
  methods: {
    openModal() {
        this.$refs["modal"].show()
    }
  }
}
</script>

<style>
.modal-footer {
    justify-content: center;
    border-top: none;
}

.exclamation {
    height: 50px;
}
</style>