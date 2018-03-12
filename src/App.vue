<template>
  <div id="app">
    <!--img src="./assets/logo.png"-->
    <!--HelloWorld/-->
    <vue-cropper :src="require('./assets/test.png')"></vue-cropper>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import VueCroper from './components/VueCropper'

export default {
  name: 'App',
  components: {
    HelloWorld,
    'vue-cropper':VueCroper
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
