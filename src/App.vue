<template>
  <div class="container">
  <h1 class="text-center mt-4">Buscando CEP - API FAKE - Via CEP</h1>
    <div class="card mb-2">
      <div class="card-body">
        <div class="mb-3">
          <label for="cep" class="form-label">Digite o CEP de pesquisa</label>
          <input type="text" class="form-control" id="cep" @keyup="verificaCep()" v-mask="'#####-###'" v-model="cep">
        </div>
      </div>
    </div>
    <div class="card" v-if="(visibilidade != 'invisible')">
      <div class="card-header">
        Informações Retiradas do Cep Informado;
      </div>
      <div class="card-body">
        <div class="row">
          <div class="col-md-6 col-sm-12">
            <p v-if="resultado.cep != ''">CEP: {{ resultado.cep }}</p>
            <p v-if="resultado.localidade != ''">Cidade: {{ resultado.localidade }}</p>
            <p v-if="resultado.complemento != ''">Complemento: {{ resultado.complemento }}</p>
            <p v-if="resultado.bairro != ''">Bairro: {{ resultado.bairro }}</p>

          </div>
          <div class="col-md-6 col-sm-12">
            <p v-if="resultado.logradouro != ''">Rua: {{ resultado.logradouro }}</p>
            <p v-if="resultado.uf != ''">Sigla do Estado: {{ resultado.uf }}</p>
            <p v-if="resultado.ibge != ''">Código no IBGE: {{ resultado.ibge }}</p>
            <p v-if="resultado.ddd != ''">DDD: {{ resultado.ddd }}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="card text-bg-danger mb-3" v-if="visibilidade2 !=='invisible'">
      <div class="card-header">Cuidado</div>
      <div class="card-body">
        <p class="card-text">O CEP digítado não foi encontrado! verifique os números e tente novamente</p>
      </div>
    </div>
  </div>
</template>
<script setup>
import { onMounted, ref, reactive } from 'vue';
import apiConf from "@/api-config.json"
import axios from "axios"
import 'vue-sweetalert2';
const baseURL = apiConf.baseURL;
const visibilidade = ref("invisible");
const visibilidade2 = ref("invisible");
const resultado = reactive({
  "cep": "",
  "logradouro": "",
  "complemento": "",
  "bairro": "",
  "localidade": "",
  "uf": "",
  "ibge": "",
  "ddd": ""
});

const verificaCep = () => {
  let valor = cep.value;
  if (valor.length == 9) {
    axios.get(baseURL + "/" + valor + "/json/").then((response) => {
      if (typeof response.data.cep !== "undefined") {
        visibilidade.value = "visible";
        resultado.cep = response.data.cep;
        resultado.complemento = response.data.complemento;
        resultado.bairro = response.data.bairro;
        resultado.localidade = response.data.localidade;
        resultado.uf = response.data.uf;
        resultado.ibge = response.data.ibge;
        resultado.ddd = response.data.ddd;
        resultado.logradouro = response.data.logradouro;
      } else {
        visibilidade2.value = "visible";
        setTimeout(() => {
          visibilidade2.value = "invisible";
        }, "3000")
      }

    }).catch((error) => {
      alert("Erro:" + JSON.stringify(Error))
    })

  } else {
    visibilidade.value = "invisible"
  }
}

onMounted(() => {
})

</script>
<script>
import { mask } from 'vue-the-mask'
export default {
  directives: { mask }
}

</script>
<style scoped>

</style>