<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',
  mounted: function () {
    console.log('mounted')
  },
  data: {
    csv: ''
  },
  methods: {
    getTicker: function () {
      let ticker = this.$refs.ticker.value.trim();
      console.log(ticker);
      var url = 'https://hello-get-n5tpywpmka-uc.a.run.app?ticker=' + ticker;
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
    <input ref="ticker" />
    <button v-on:click="getTicker">Get stock</button>
    <p>{{ csv }}</p>
  </div>
</template>

<style></style>
