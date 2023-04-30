<script setup>
import { ref } from 'vue';

const serial = ref('');

function checkSerial() {
  console.log(serial.value)
}

</script>

<template>
  <main class="container">
    <form id="form" @submit.prevent="onSumit">
      <label for="serial">Codi de la T-casual
        <input required id="serial" type="text" placeholder="Indroueix el serial aqui" v-model="serial"/>
        <small>Per exemple: 013501 1212090009</small>
      </label>

      <button @click="checkSerial()" id="submit">Decode !</button>
    </form>
    <div id="results">
      
    </div>
  </main>
  <footer class="footer">
    <small>Made with &#9829; by <a href="https://github.com/Oloxx">Oloxx</a></small>
  </footer>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 96vh;
}

.footer {
  bottom: 0;
  opacity: 0.69;
  text-align: center;
}
</style>