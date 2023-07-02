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
let ipA = '';
let ipB = '';
const array = ref([])
const subred = ref(0);
const subRedValido = ref(0);
let valorA = 0;

const generar = () => {
  quitar();
  subred.value = Math.pow(2, quito.value);
  rango = 256/subred.value;
  
  verificarClase();
  
  claseIP.value = "La clase IP: "+claseIP.value
};

const verificarClase=()=>{
  if(ip1.value<128 && ip1.value>0){
    claseIP.value="A"
    valorA=24;
    generarSubA();
  }else if(ip1.value<192 && ip1.value >127){
    claseIP.value="B"
    valorA=16
    generarSubB();
  }else if(ip1.value<224 && ip1.value>191){
    claseIP.value="C"
    valorA = 8;
    generarSubC();
  }else if(ip1.value<226 && ip1.value>223){
    claseIP.value="D - Aqui no se puede generar subredes"
  }else{
    claseIP.value="INVALIDA"
  }
}
//falta optimizar
const quitar=()=>{
  if(numSubRedes.value >0 && numSubRedes.value <= 2){
    quito.value = 1
  }else if(numSubRedes.value > 2 &&numSubRedes.value <=4){
    quito.value = 2
  }else if(numSubRedes.value > 4 &&numSubRedes.value <=8){
    quito.value = 3
  }else if(numSubRedes.value > 8 &&numSubRedes.value <=16){
    quito.value = 4
}else if(numSubRedes.value > 16 &&numSubRedes.value <=32){
  quito.value = 5
}else if(numSubRedes.value > 32 &&numSubRedes.value <=64){
  quito.value = 6
}else if(numSubRedes.value > 64 &&numSubRedes.value <=128){
  quito.value = 7
}else if(numSubRedes.value > 128 &&numSubRedes.value <=256){
  quito.value = 8
}
}

const generarSubC=()=>{
  
  ipC = ip1.value+'.'+ip2.value+'.'+ip3.value+'.'
  
  console.log(subred.value)

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

  ipA = ip1.value+'.';
  
  subRedValido.value = subred.value-2;

  
  for(let i = 0;i<subred.value;i++){
    
    indice = contador+1;
    despues = contador +rango-1;
    
    array.value.push({
      Nro: i+1,
      subRed: ipA+''+contador+'.'+ip3.value+'.'+ip4.value,
      ipsConfigurables: ipA+contador+'.'+0+'.'+1+'    -     '+ipA+despues+'.'+255+'.'+254,
      broadcast: ipA+despues+".255"+"255",
    })
    contador = contador+rango; 
  }
}

const generarSubB=()=>{

  ipB = ip1.value+'.'+ip2.value+'.';
  
  subRedValido.value = subred.value-2;

  
  for(let i = 0;i<subred.value;i++){
    
    indice = contador+1;
    despues = contador +rango-1;
    
    array.value.push({
      Nro: i+1,
      subRed: ipB+''+contador+'.'+ip4.value,
      ipsConfigurables: ipB+''+contador+'.'+1+'    -     '+ipB+despues+'.'+254,
      broadcast: ipB+despues+".255",
    })
    contador = contador+rango; 
  }
}

const refrescar=()=>{
  location.reload();
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
      </div>
      <br />
      <button type="submit" @click="generar" class="btn btn-primary">Generar</button>
      <button type="submit" @click="refrescar" class="btn btn-success">Resetear</button>
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