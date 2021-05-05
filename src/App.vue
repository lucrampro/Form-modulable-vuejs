<template>
  <div id="app">
    <div class="container--formlaire">
      <Formulaire />
    </div>
  </div>
</template>

<script>
  import Formulaire from './components/Formulaire.vue'

  export default {
    name: 'App',
    components: {
      Formulaire
    }
  }
</script>

<style>
  #app {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .container--formlaire {
    width: 500px;
  }
</style>