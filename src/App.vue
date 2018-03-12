<template>
  <div id="app">
    <vue-cropper :src="require('./assets/test.jpg')" ref="crop"></vue-cropper>
    <button @click="myImage">Click here!</button>
  </div>
</template>

<script>
import VueCroper from './components/VueCropper';

export default {
  name: 'App',
  components: {
      'vue-cropper': VueCroper
  },
    methods: {
      myImage() {
          console.log(this.$refs.crop.getCroppedCanvas().toDataURL());
      }
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
