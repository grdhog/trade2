<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',
  mounted: function () {
    console.log('mounted');
    let aDate = new Date();
    aDate.setFullYear(aDate.getFullYear() - 1);
    this.period1 = aDate.getTime();
    this.period2 = Date.now();    
  },
  data: {
    csv: '',
    period1: 0,
    period2: 0,
    closing: []
  },
  methods: {
    convertCSV: function(){
      const THE_DATE = 0;
      const ADJUSTED_CLOSE = 5;
      const lines = this.csv.split('\n');
      console.log('num lines', lines.length);
      const headers = lines.shift();
      console.log('headers', headers);
      const array = [];
      for (const line of lines){
        let parts = line.split(',');
        array.push({'date': parts[THE_DATE], 'price': parts[ADJUSTED_CLOSE]});
      }
      this.closing = array;
    },
    getTicker: function () {
      let ticker = this.$refs.ticker.value.trim();    
      let params = {};
      params.period1 = this.period1;
      params.period2 = this.period2;
      params.ticker = ticker;
      const qs = '?' + new URLSearchParams(params).toString();
      var url = 'https://hello-get-n5tpywpmka-uc.a.run.app' + qs;
      console.log('url', url)
      var oReq = new XMLHttpRequest()
      var that = this
      oReq.addEventListener('load', function () {
        that.csv = this.responseText;
        that.convertCSV();
      })
      oReq.addEventListener('error', function () {
        console.error('API not working!')
      })
      oReq.open('GET', url)
      oReq.send()
    },
  },
})
</script>

<template>
  <div id="app">
    <p>period1: {{ period1 }} period2: {{ period2 }}</p>
    <input ref="ticker" />
    <button v-on:click="getTicker">Get stock</button>
    <p>{{ csv }}</p>
    <p>{{ closing }}</p>
  </div>
</template>

<style></style>
