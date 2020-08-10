<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import Cesium from 'cesium/Cesium' //from 中 cesium 指向的是vue.config.js中 alias对象中的别名
// noinspection ES6UnusedImports
// eslint-disable-next-line no-unused-vars
import widget from 'cesium/Widgets/widgets.css'
import HelloWorld from './components/Earth/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  padding:0px;
  margin: 0px;
  position: absolute;
  top:0px;
  left: 0px;
  right: 0px;
  bottom: 0px;
}
</style>
