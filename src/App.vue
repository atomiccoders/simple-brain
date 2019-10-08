<template>
  <div id="app">
    <h2>Simple Brain Model</h2>
    <p>
      The application looks on the canvas where you can draw a digit and then
      returns a digit that you draw.
    </p>
    <Canvas />
    <Results />
  </div>
</template>

<script>
import Canvas from './components/Canvas.vue'
import Results from './components/Results.vue'

export default {
  name: 'app',
  components: {
    Canvas,
    Results,
  },
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
