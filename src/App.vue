<template>
  <div id="app">
    <div class="container">
      <Home/>

    </div>
  </div>
</template>

<script>

  import Home from './Components/Home.vue'

export default {
  name: 'app',
  components: {
    Home
  }
}
</script>

<style lang="scss">

  #app {
    margin-bottom: 5px;
    margin-top: 5px;
    font-family: Helvetica, sans-serif;
  }

</style>
