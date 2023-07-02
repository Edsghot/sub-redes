<script setup>
import { normalizeClass, ref } from "vue";
const ip1 = ref(200);
const ip2 = ref(200);
const ip3 = ref(200);
const ip4 = ref(0);
let contador = 0;
  let indice = 0;
  let despues = 0;
const numSubRedes = ref(0);
const claseIP = ref(" ");
const quito = ref(0)
let rango = 0;
let ipC = '';
const array = ref([])
const subred = ref(0);
const subRedValido = ref(0);

const generar = () => {
  verificarClase();
  generarSubC();
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


const generarSubC=()=>{
  subred.value = Math.pow(2, quito.value);
  rango = 256/subred.value;
  ipC = ip1.value+'.'+ip2.value+'.'+ip3.value+'.'
  
  console.log(rango)

  subRedValido.value = subred.value-2;
  
  for(let i = 0;i<subred.value;i++){
    
    indice = contador+1;
    despues = contador +rango-2;
    
    array.value.push({
      Nro: i+1,
      subRed: ipC+''+contador,
      ipsConfigurables: ipC+indice+'    -     '+ipC+despues,
      broadcast: ipC+(despues +1),
    })
    contador = contador+rango; 
  }
}
const generarSubA=()=>{
  subred.value = Math.pow(2, quito.value);
  rango = 256/subred.value;
  ipC = ip1.value+'.'+ip2.value+'.'+ip3.value+'.'
  
  console.log(rango)

  subRedValido.value = subred.value-2;
  
  for(let i = 0;i<subred.value;i++){
    
    indice = contador+1;
    despues = contador +rango-2;
    
    array.value.push({
      Nro: i+1,
      subRed: ipC+''+contador,
      ipsConfigurables: ipC+indice+'    -     '+ipC+despues,
      broadcast: ipC+(despues +1),
    })
    contador = contador+rango; 
  }
}

const generarSubB=()=>{
  subred.value = Math.pow(2, quito.value);
  rango = 256/subred.value;
  ipC = ip1.value+'.'+ip2.value+'.'+ip3.value+'.'
  
  console.log(rango)

  subRedValido.value = subred.value-2;
  
  for(let i = 0;i<subred.value;i++){
    
    indice = contador+1;
    despues = contador +rango-2;
    
    array.value.push({
      Nro: i+1,
      subRed: ipC+''+contador,
      ipsConfigurables: ipC+indice+'    -     '+ipC+despues,
      broadcast: ipC+(despues +1),
    })
    contador = contador+rango; 
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
    <div class="container">
    <table class="table">
      <thead class="thead-dark">
        <tr>
          <th>Nro</th>
          <th>Subred</th>
          <th>IPs Configurables</th>
          <th>Broadcast</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in array" :key="index">
          <td>{{ item.Nro }}</td>
          <td>{{ item.subRed }}</td>
          <td>{{ item.ipsConfigurables }}</td>
          <td>{{ item.broadcast }}</td>
        </tr>
      </tbody>
    </table>
  </div>
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