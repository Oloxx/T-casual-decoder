<script setup>
import { ref } from 'vue';
import data from './json/estacions.json';

const serial = ref('');
const linea = ref('??');
const estacio = ref('???');
const zona = ref('??');
const diaMes = ref('??/??');
const horaMin = ref('??:??');
const viatges = ref('??');
const vLinea = ref(null);
const vEstacio = ref(null);
const vZona = ref(null);
const vDiaMes = ref(null);
const vHoraMin = ref(null);
const vViatges = ref(null);

function checkSerial() {
  const codi = serial.value.replace(/\s/g, '');
  linea.value = checkLinea(codi.substring(0, 2));
  estacio.value = checkEstacio(codi.substring(1, 4));
  zona.value = checkZona(codi.substring(4, 6));
  diaMes.value = checkDiaMes(codi.substring(6, 8), codi.substring(8, 10));
  horaMin.value = checkHoraMin(codi.substring(10, 12), codi.substring(12, 14));
  viatges.value = checkViatges(codi.substring(14, 16));
}

function checkLinea(lineaMetro) {
  vLinea.value = false;
  switch (lineaMetro) {
    case '01':
      return 'L1';
    case '02':
      return 'L2';
    case '03':
      return 'L3';
    case '04':
      return 'L4';
    case '05':
      return 'L5';
    case '09':
      return 'L9/L10';
    default:
      vLinea.value = true;
      return '??';
  }
}

function checkEstacio(EstacioMetro) {
  const findEstacio = data.estacions.find(e => e.codi === EstacioMetro);
  if (!findEstacio) {
    vEstacio.value = true;
    return '???';
  }
  vEstacio.value = false;
  return findEstacio.nom;
}

function checkZona(zona) {
  if (zona != '01') {
    vZona.value = true;
    return '??';
  }
  vZona.value = false;
  return zona;
}

function checkDiaMes(dia, mes){
  const date = new Date(null, mes - 1, dia);
  let month = date.getMonth() + 1;
  let day = date.getDate();
  if (month == mes && day == dia) {
    day = dia;
    month = mes;
    vDiaMes.value = false;
    return `${day}/${month}`;
  }

  vDiaMes.value = true;
  return `??/??`;
}

function checkHoraMin(hora, minut) {
 let hour = hora;
 let minute = minut;
 vHoraMin.value = false;
 if (hora > 24) {
  hour = '??';
  vHoraMin.value = true;
 }

 if (minut > 59) {
  minute = '??';
  vHoraMin.value = true;
 }
 
 return `${hour}:${minute}`;
}

function checkViatges(viatges) {
  if (viatges > 9) {
    vViatges.value = true;
    return '??';
  }
  vViatges.value = false;
  return viatges;
}

</script>

<template>
  <main class="container container-page">
    <form id="form" @submit.prevent="onSumit" style="margin-top: 100px;">
      <label for="serial">Codi de la T-casual
        <input required id="serial" type="text" placeholder="Indroueix el serial aqui" v-model="serial" @keyup="checkSerial()"/>
        <small>Per exemple: 013501 1212090009</small>
      </label>
    </form>
    <div class="parent">
      <div class="a">
        <label for="linea">Linea:
          <input type="text" id="linea" readonly v-model="linea" :aria-invalid="vLinea">
        </label>
      </div>
      <div class="b">
        <label for="estacio">Estació:
          <input type="text" id="estacio" readonly v-model="estacio" :aria-invalid="vEstacio">
        </label>
      </div>
      <div class="c">
        <label for="zona">Zona:
          <input type="text" id="zona" readonly v-model="zona" :aria-invalid="vZona">
        </label>
      </div>

      <div class="d">
        <label for="dia">Dia i Mes:
          <input type="text" id="dia" readonly v-model="diaMes" :aria-invalid="vDiaMes">
        </label>
      </div>
      <div class="e">
        <label for="hora">Hora i minut:
          <input type="text" id="hora" readonly v-model="horaMin" :aria-invalid="vHoraMin">
        </label>
      </div>
      <div class="f">
        <label for="viatges">Viatges:
          <input type="text" id="viatges" readonly v-model="viatges" :aria-invalid="vViatges">
        </label>
      </div>
    </div>
  </main>
  <footer class="footer">
    <small>Made with &#9829; by <a href="https://github.com/Oloxx">Oloxx</a></small>
  </footer>
</template>

<style scoped>
.container-page {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  height: 96vh;
}

.footer {
  bottom: 0;
  opacity: 0.69;
  text-align: center;
}

.parent {
  display: grid; 
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr; 
  grid-template-rows: 1fr 1fr; 
  gap: 0px 15px; 
  grid-template-areas: 
    "a a b b b b c"
    "d d d e e e f"; 
max-width: 800px;
margin-top: 100px;
}

.a { grid-area: a; }
.b { grid-area: b; }
.c { grid-area: c; }
.d { grid-area: d; }
.e { grid-area: e; }
.f { grid-area: f; }
</style>