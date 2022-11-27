<template>
  <div class="app">
  <img class="logo" alt="Vue logo" src="./assets/logo.png">
  <MainBoard />
  </div>
</template>

<script>
import MainBoard from './components/MainBoard.vue';

export default {

  components: {
    MainBoard
  }
}

</script>

<style>
.app {
  margin-top: 60px;
  text-align: center;

  user-select: none; /* supported by Chrome and Opera */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
}
.logo {
  width: 100px;
}
</style>
