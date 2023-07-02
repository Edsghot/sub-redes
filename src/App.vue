<script setup>
import { normalizeClass, ref } from "vue";
const ip1 = ref(0);
const ip2 = ref(0);
const ip3 = ref(0);
const ip4 = ref(0);
const numSubRedes = ref(0);
const claseIP = ref(" ");
const quito = ref(0)

const generar = () => {
  verificarClase();

  claseIP.value = "La clase IP: "+claseIP.value
};

const verificarClase=()=>{
  if(ip1.value<128 && ip1.value>0){
    claseIP.value="A"
  }else if(ip1.value<192 && ip1.value >127){
    claseIP.value="B"
  }else if(ip1.value<224 && ip1.value>191){
    claseIP.value="C"
  }else if(ip1.value<226 && ip1.value>223){
    claseIP.value="A"
  }else{
    claseIP.value="INVALIDA"
  }
}



</script>

<template>
  <div class="container">
    <form onsubmit="event.preventDefault();">
      <div class="form-group">
        <h4 class="mt-2">Digite su IP:</h4>
        <div class="row">
          <div class="col-1">
            <input
              type="number"
              class="form-control form-control-sm rounded col-1"
              v-model="ip1"
            />
          </div>
          <div class="col-1">
            <input
              type="number"
              class="form-control form-control-sm rounded col-1"
              v-model="ip2"
            />
          </div>
          <div class="col-1">
            <input
              type="number"
              class="form-control form-control-sm rounded col-1"
              v-model="ip3"
            />
          </div>
          <div class="col-1">
            <input
              type="number"
              class="form-control form-control-sm rounded col-1"
              v-model="ip4"
            />
          </div>
        </div>
        <h2 class="mt-3">{{ ip1 }}.{{ ip2 }}.{{ ip3 }}.{{ ip4 }}</h2>
      </div>
      <div class="form-group">
        <h4>Digite la cantidad de subredes que desea:</h4>
        <div class="col-1">
          <input
            type="number"
            v-model="numSubRedes"
            class="form-control form-control-sm rounded col-1"
          />
        </div>
        <h4>QUITO:</h4>
        <div class="col-1">
          <input
            type="number"
            v-model="quito"
            class="form-control form-control-sm rounded col-1"
          />
        </div>
      </div>
      <br />
      <button type="submit" @click="generar" class="btn btn-primary">Generar</button>
    </form>

    <h3>{{ claseIP }}</h3>
  </div>
</template>

<style>
body {
  background-color: grey;
}
.input-custom-size {
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  height: 2.25rem;
}
</style>