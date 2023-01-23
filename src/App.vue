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
    period2: 0  
  },
  methods: {
    getTicker: function () {
      let ticker = this.$refs.ticker.value.trim();
      console.log(ticker);

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
  </div>
</template>

<style></style>
