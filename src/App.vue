<template>
  <div id="app">
    <h1 id="title"> ECommerce sales by merchandise category 1999-2015 </h1>
    <KabumChart :ecommerceSalesData="loadData" />
  </div>
</template>

<script>
import KabumChart from './components/KabumChart.vue';
import * as d3 from 'd3';

export default {
  name: 'App',
  components: {
    KabumChart
  },
  data(){
    return {
      loadData: []
    };
  },
  async created() {
    console.log("App loaded");
    this.fetchData();
  },
  methods: {
    async fetchData() {
      let url = "http://localhost:8080/testapi/analytics";
      let ecommerceSalesData = await d3.json(url, {
        method: 'POST',
        mode: 'cors',
        headers: {
          'Content-Type': 'application/json'
        }
      });
      this.loadData = ecommerceSalesData;
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
#title{
  font-weight: bold;
}
</style>
