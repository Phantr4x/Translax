<template lang="jade">
  .translax-input-box
    input#translax-input.translax-input(
      type='text'
      name='translax-input'
      placeholder='在此輸入要翻譯的文本...'
      v-model='query'
    )
</template>

<script>
export default {
  props: ['query']
}
</script>

<style lang="sass">
.translax-input-box {
  width: 100%;
}
input.translax-input {
  width: 100%;
  margin: 0;
  padding: 0 .5em;
  border: none;
  outline: none;
  color: #343434;
  font: normal 24px/2.4 "Helvetica Neue", Arial, sans-serif;
  box-shadow: 0 2px 5px 0 rgba(0,0,0,0.16),0 2px 10px 0 rgba(0,0,0,0.12);
}
</style>
