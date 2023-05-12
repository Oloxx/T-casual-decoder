<script setup>
import { ref } from 'vue';
import stations from './json/stations.json';
import lines from './json/lines.json';

const serial = ref('');
const linea = ref('??');
const estacio = ref('???');
const zona = ref('??');
const diaMes = ref('??/??');
const horaMin = ref('??:??');
const viatges = ref('??');
const vInput = ref(null);
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
  
  vInput.value = null;

  if (codi.length > 16) {
    vInput.value = true;
  }
}

function checkLinea(lineaMetro) {
  if (lineaMetro) {
    const findLine = lines.lines.find(l => l.codi === lineaMetro);
    vLinea.value = !findLine;

    return findLine ? findLine.nom : '??';
  }
  return '??';
}


function checkEstacio(EstacioMetro) {
  if (EstacioMetro) {
    const findEstacio = stations.stations.find(e => e.codi === EstacioMetro);
    vEstacio.value = !findEstacio;

    return findEstacio ? findEstacio.nom : '???';
  }
  return '???';
}

function checkZona(zona) {
  if (zona) {
    vZona.value = zona !== '01';

    return vZona.value ? '??' : zona;
  }
  return '??';
}

function checkDiaMes(dia, mes) {
  if (dia && mes) {
    const date = new Date(null, mes - 1, dia);
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');

    vDiaMes.value = !(month === mes && day === dia);

    return vDiaMes.value ? '??/??' : `${day}/${month}`;
  }
  return '??/??';
}

function checkHoraMin(hora, minut) {
  if (hora && minut) {
    vHoraMin.value = false;

    if (hora > 24 || minut > 59) {
      vHoraMin.value = true;
    }
    const hour = (hora > 24) ? '??' : hora;
    const minute = (minut > 59) ? '??' : minut;

    return `${hour}:${minute}`;
  }
  return '??:??';
}


function checkViatges(viatges) {
  if (viatges) {
    const viatgesI = parseInt(viatges);
    vViatges.value = !(viatgesI <= 9 && viatgesI >= 0);

    return vViatges.value ? '??' : viatges;
  }
  return '??';
}

</script>

<template>
  <main class="container container-page">
    <form id="form" @submit.prevent="onSubmit" style="margin-top: 100px;">
      <label for="serial">Codi de la T-casual
        <input required id="serial" type="text" placeholder="Indroueix el serial aqui" v-model="serial" @keyup="checkSerial" :aria-invalid="vInput"/>
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