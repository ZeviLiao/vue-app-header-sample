<template>
  <div id="app">
    <DemoPage/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import DemoPage from './views/demoPage.vue'

export default {
  name: 'app',
  components: {
    DemoPage
  }
}
</script>

<style>
#app {
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif; */
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #333;
  /* margin-top: 60px; */
}
</style>
