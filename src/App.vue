<template>
  <div id="app">
    <Header/>
    <v-container>
      <v-row justify="center">
        <Barchart/>
        <RingChart/>
        <Currencychart/>
      </v-row>
    </v-container>
  </div>
</template>

<script>
  import Header from './components/Header.vue'
  import Barchart from './components/Barchart.vue'
  import RingChart from './components/RingChart.vue'
  import Currencychart from './components/Currencychart.vue'
export default {
  name: 'App',
  components: {
    Header,
    Barchart,
    RingChart,
    Currencychart
  },
  data() {
    return {
    };
  },

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

.chart div {
  font: 10px sans-serif;
  background-color: steelblue;
  text-align: right;
  padding: 3px;
  margin: 1px;
  color: white;
}
.container-chart {
  /* width: 50%; */
  margin: auto;
}
</style>
