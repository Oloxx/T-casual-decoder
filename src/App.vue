<script setup>
import { ref } from 'vue';
import data from './json/estacions.json';

const serial = ref('');
const linea = ref('???');
const estacio = ref('???');
const zona = ref('???');
const dia = ref('???');
const mes = ref('???');
const hora = ref('???');
const minut = ref('???');
const viatges = ref('???');

function checkSerial() {
  let codi = serial.value.replace(/\s/g, '');
  if (codi.length === 16) {
    linea.value = checkLinea(codi.substring(0, 2));
    estacio.value = checkEstacio(codi.substring(1, 4));
    zona.value = codi.substring(4, 6);
    dia.value = checkDia(codi.substring(6, 8), codi.substring(8, 10));
    mes.value = checkMes(codi.substring(8, 10));
    hora.value = codi.substring(10, 12);
    minut.value = codi.substring(12, 14);
    viatges.value = codi.substring(14, 16);
  }
}

function checkLinea(lineaMetro) {
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
      return '???';
  }
}

function checkEstacio(EstacioMetro) {
  const findEstacio = data.estacions.find(e => e.codi === EstacioMetro);
  if (!findEstacio) return '???';

  return findEstacio.nom;
}

function checkDia(dia, mes){
  const date = new Date(null, mes - 1, dia);
  if (date.getMonth() + 1 != mes && date.getDate() != dia) {
    return '???';
  }

  return dia;
}

function checkMes(mes){
  if (mes > 12) return '???';
  return mes;
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
      <div class="div1">
        <label for="linea" class="">Linea:
          <input type="text" id="linea" disabled v-model="linea">
        </label>
      </div>
      <div class="div2">
        <label for="estacio" class="">Estació:
          <input type="text" id="estacio" disabled v-model="estacio" >
        </label>
      </div>
      <div class="div3">
        <label for="zona">Zona:
          <input type="text" id="zona" disabled v-model="zona">
        </label>
      </div>

      <div class="div4">
        <label for="dia">Dia:
          <input type="text" id="dia" disabled v-model="dia">
        </label>
      </div>
      <div class="div5">
        <label for="mes">Mes:
          <input type="text" id="mes" disabled v-model="mes">
        </label>
      </div>
      <div class="div6">
        <label for="hora">Hora:
          <input type="text" id="hora" disabled v-model="hora">
        </label>
      </div>
      <div class="div7">
        <label for="minut">Minut:
          <input type="text" id="minut" disabled v-model="minut">
        </label>
      </div>
      <div class="div8">
        <label for="viatges">Viatges:
          <input type="text" id="viatges" disabled v-model="viatges">
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
grid-template-columns: repeat(12, 1fr);
grid-template-rows: repeat(3, 1fr);
grid-column-gap: 12px;
max-width: 800px;
margin-top: 100px;
}

.div1 { grid-area: 1 / 1 / 2 / 4; }
.div2 { grid-area: 1 / 4 / 2 / 10; }
.div3 { grid-area: 1 / 10 / 2 / 13; }
.div4 { grid-area: 2 / 1 / 3 / 4; }
.div5 { grid-area: 2 / 4 / 3 / 7; }
.div6 { grid-area: 2 / 7 / 3 / 10; }
.div7 { grid-area: 2 / 10 / 3 / 13; }
.div8 { grid-area: 3 / 5 / 4 / 9; }
</style>