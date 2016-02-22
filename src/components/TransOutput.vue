<template lang="jade">
  .translax-output
    pre {{ result }}
</template>

<script>
import md5 from 'js-md5'

export default {
  props: ['query'],
  data () {
    return {
      result: ''
    }
  },
  watch: {
    'query': 'youdao'
  },
  methods: {
    baidu (txt) {
      const baidu_url = 'http://api.fanyi.baidu.com/api/trans/vip/translate'
      const appid = '20151211000007661'
      const key = 'r4pYIDpysZQz9viTZmSh'
      var current = new Date()
      var salt = current.getTime()
      var query = txt
      var hash = md5.hex(appid + query + salt + key)

      this.$http.jsonp(baidu_url, {
        q: query,
        from: 'auto',
        to: 'zh',
        appid: appid,
        salt: salt,
        sign: hash
      }).then(function (response) {
        var json = response.data
        console.log(json)
        this.result = json.trans_result[0].dst
      }, function (response) {
        // handle error
      })
    },
    youdao (txt) {
      const youdao_url = 'http://fanyi.youdao.com/openapi.do'
      this.$http.jsonp(youdao_url, {
        keyfrom: 'Translax',
        key: '1345532443',
        type: 'data',
        doctype: 'jsonp',
        callback: 'show',
        version: '1.1',
        q: txt
      }).then(function (response) {
        var json = response.data
        var basic = json.basic.explains.join('\n')
        this.result = basic
        console.log(basic)
      }, function (response) {
        // handle error
      })
    }
  }
}
</script>

<style lang="sass">
.translax-output {
  display: block;
  padding: .5em;
  font: normal 24px/1.5 "Helvetica Neue", Arial, sans-serif;
}
</style>
