<template>
  <div id="translax">
    <trans-input :query.sync='query'></trans-input>
    <trans-output :query='query'></trans-output>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'

import TransInput from './components/TransInput'
import TransOutput from './components/TransOutput'

Vue.use(VueResource)

export default {
  el: '#translax',
  data () {
    return {
      query: ''
    }
  },
  components: {
    TransInput,
    TransOutput
  }
}
</script>

<style lang="sass">
@import 'assets/lib/css-wipe.css';
body {
  margin: 0;
  padding: 0;
  background: #f4f4f4;
  font-family: Helvetica, sans-serif;
}
</style>
