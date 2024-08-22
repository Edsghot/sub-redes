<script setup>
import { ref } from 'vue';

const ip1 = ref(123);
const ip2 = ref(0);
const ip3 = ref(5);
const ip4 = ref(4);
let contador = 0;
let indice = 0;
let despues = 0;
const numSubRedes = ref(16);
const claseIP = ref("");
const quito = ref(3);
let rango = 0;
let ipC = '';
let ipA = '';
let ipB = '';
const array = ref([]);
const subred = ref(0);
const subRedValido = ref(0);
let mascara = '';
let mascaraIp = 0;

// Información adicional
const networkIP = ref('');
const borrowedBits = ref(0);
const cidr = ref(0);
const salto = ref(0);
const ipsPorSubRed = ref(0);

const generar = () => {
  array.value = []; // Resetear la tabla
  contador = 0; // Reiniciar contador
  quitar();
  subred.value = Math.pow(2, quito.value);
  rango = 256 / subred.value;

  verificarClase();

  calcularInformacionAdicional();

  claseIP.value = "La clase IP: " + claseIP.value;
};

const verificarClase = () => {
  if (ip1.value < 128 && ip1.value > 0) {
    claseIP.value = "A";
    mascara = '255.' + mascaraIp + '.0.0';
    networkIP.value = `${ip1.value}.0.0.0`;
    generarSubA();
  } else if (ip1.value < 192 && ip1.value > 127) {
    claseIP.value = "B";
    mascara = '255.255.' + mascaraIp + '.0';
    networkIP.value = `${ip1.value}.${ip2.value}.0.0`;
    generarSubB();
  } else if (ip1.value < 224 && ip1.value > 191) {
    claseIP.value = "C";
    mascara = '255.255.255.' + mascaraIp;
    networkIP.value = `${ip1.value}.${ip2.value}.${ip3.value}.0`;
    generarSubC();
  } else if (ip1.value < 226 && ip1.value > 223) {
    claseIP.value = "D - Aquí no se pueden generar subredes";
  } else {
    claseIP.value = "INVALIDA";
  }
};

const quitar = () => {
  if (numSubRedes.value > 0 && numSubRedes.value < 2) {
    quito.value = 1;
    mascaraIp = 192;
  } else if (numSubRedes.value >= 2 && numSubRedes.value < 4) {
    quito.value = 2;
    mascaraIp = 224;
  } else if (numSubRedes.value >= 4 && numSubRedes.value < 8) {
    quito.value = 3;
    mascaraIp = 240;
  } else if (numSubRedes.value >= 8 && numSubRedes.value < 16) {
    quito.value = 4;
    mascaraIp = 248;
  } else if (numSubRedes.value >= 16 && numSubRedes.value < 32) {
    quito.value = 5;
    mascaraIp = 248;
  } else if (numSubRedes.value >= 32 && numSubRedes.value < 64) {
    quito.value = 6;
    mascaraIp = 252;
  } else if (numSubRedes.value >= 64 && numSubRedes.value < 128) {
    quito.value = 7;
    mascaraIp = 254;
  } else if (numSubRedes.value >= 128 && numSubRedes.value < 256) {
    quito.value = 8;
    mascaraIp = 255;
  }
};

const calcularInformacionAdicional = () => {
  borrowedBits.value = quito.value;
  cidr.value = 8 + borrowedBits.value; // Ejemplo para clase A, ajustar según clase IP
  salto.value = 256 / subred.value;
  ipsPorSubRed.value = Math.pow(2, (32 - cidr.value)) - 2;
};

const generarSubC = () => {
  ipC = ip1.value + '.' + ip2.value + '.' + ip3.value + '.';

  subRedValido.value = subred.value - 2;

  for (let i = 0; i < subred.value; i++) {
    indice = contador + 1;
    despues = contador + rango - 2;

    array.value.push({
      Nro: i + 1,
      subRed: ipC + '' + contador,
      ipsConfigurables: ipC + indice + '    -     ' + ipC + despues,
      broadcast: ipC + (despues + 1),
    });
    contador = contador + rango;
  }
};

const generarSubA = () => {
  ipA = ip1.value + '.';

  subRedValido.value = subred.value - 2;

  for (let i = 0; i < subred.value; i++) {
    indice = contador + 1;
    despues = contador + rango - 1;

    array.value.push({
      Nro: i + 1,
      subRed: ipA + '' + contador + '.' + ip3.value + '.' + ip4.value,
      ipsConfigurables: ipA + contador + '.' + 0 + '.' + 1 + '    -     ' + ipA + despues + '.' + 255 + '.' + 254,
      broadcast: ipA + despues + ".255.255",
    });
    contador = contador + rango;
  }
};

const generarSubB = () => {
  ipB = ip1.value + '.' + ip2.value + '.';

  subRedValido.value = subred.value - 2;

  for (let i = 0; i < subred.value; i++) {
    indice = contador + 1;
    despues = contador + rango - 1;

    array.value.push({
      Nro: i + 1,
      subRed: ipB + '' + contador + '.' + ip4.value,
      ipsConfigurables: ipB + '' + contador + '.' + 1 + '    -     ' + ipB + despues + '.' + 254,
      broadcast: ipB + despues + ".255",
    });
    contador = contador + rango;
  }
};

const refrescar = () => {
  location.reload();
};
</script>



<template>
  <div class="container">
    <br/>
    <img src="./assets/logoPagina.png" width="300">
    <br/><br/>
    <form @submit.prevent>
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

    <h3>MASCARA: {{ mascara }}</h3>
    <h3>{{ claseIP }}</h3>
    <h3>Subredes: {{ subred }}</h3>
    <h3>Subredes válidas: {{ subRedValido }}</h3>

    <!-- Información adicional -->
    <h4>IP de red: {{ networkIP }}</h4>
    <h4>Bits prestados: {{ borrowedBits }}</h4>
    <h4>Máscara de subred (CIDR): /{{ cidr }}</h4>
    <h4>Número de IPs por subred: {{ ipsPorSubRed }}</h4>
    <h4>Salto entre subredes: {{ salto }}</h4>

    <div class="table-container">
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
.container {
  border: 1px solid #fff;
  padding: 20px;
  border-radius: 8px;
  background-color: #1a2b5f;
  max-height: 90vh; /* Ajusta la altura máxima de la caja */
  overflow-y: auto; /* Agrega un scroll vertical si el contenido excede la altura */
}

body {
  background-color: #101652;
  font-family: 'Arial', sans-serif;
  color: #fff;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

h2 {
  color: #207178;
}

h4, h3 {
  color: #edeccf;
}

.input-custom-size {
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  height: 2.25rem;
}

.form-control {
  background-color: #2a3f73;
  color: #fff;
  border: 1px solid #3a4e8c;
  width: 100px; /* Ajusta el ancho de los inputs */
}

.form-control-sm {
  font-size: 0.875rem;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
}

.btn {
  margin: 5px;
}

.btn-primary {
  background-color: #3467c1;
  border-color: #345ea1;
}

.btn-primary:hover {
  background-color: #2d5aa5;
  border-color: #2d5295;
}

.btn-success {
  background-color: #28a745;
  border-color: #218838;
}

.btn-success:hover {
  background-color: #218838;
  border-color: #1e7e34;
}

.table-container {
  max-height: 400px; /* Ajusta este valor según necesites */
  overflow-y: auto;
  margin-top: 20px;
}

.table {
  width: 100%;
  margin-top: 20px;
  border-collapse: collapse;
}

.table th, .table td {
  border: 1px solid #345ea1;
  padding: 8px;
  text-align: center;
}

.table thead {
  background-color: #3a4e8c;
  color: #fff;
}

.table tbody tr:nth-child(odd) {
  background-color: #2a3f73;
}

.table tbody tr:nth-child(even) {
  background-color: #1e3059;
}

.table tbody tr:hover {
  background-color: #3a4e8c;
  color: #fff;
}

.SegundoLado {
  margin-top: 20px;
  border: 2px solid #edeccf;
  padding: 10px;
  color: #edeccf;
  background-color: #2a3f73;
}
</style>
