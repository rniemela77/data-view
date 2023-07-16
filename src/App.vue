<template>
  <LineChart :data="data"/>
</template>

<script>
import LineChart from './components/LineChart.vue'

export default {
  name: 'App',
  components: {
    LineChart
  },
  data() {
    return {
      data: [10, 20, 30, 25, 35, 40, 30, 45, 50, 55],
    }
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
  margin-top: 60px;
}
</style>
