<template lang="jade">
  .translax-output
    h1.query-output {{ initialQuery }}
    .phonetic-output
      p.uk-phonetic-output {{ UKPhonetic }}
      p.us-phonetic-output {{ USPhonetic }}
    .basic-output
      h2 「基礎釋義」
      p(v-for='item in basicExp') {{ basicExp[$index] }}
    .web-output
      h2 「網路釋義」
      ul
        li(v-for='(index, item) in webExp')
          h3 {{ webExp[index].key }}
          p(v-for='(i, val) in webExp[index].value') {{ webExp[index].value[i] }}
</template>

<script>
import md5 from 'js-md5'

export default {
  props: ['query'],
  data () {
    return {
      initialQuery: '',
      UKPhonetic: '',
      USPhonetic: '',
      basicExp: [],
      webExp: []
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
        this.basicExp = json.trans_result[0].dst
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
        this.initialQuery = json.query
        this.UKPhonetic = json.basic['uk-phonetic'] !== undefined ? '英 [ ' + json.basic['uk-phonetic'] + ' ]' : ''
        this.USPhonetic = json.basic['us-phonetic'] !== undefined ? '美 [ ' + json.basic['us-phonetic'] + ' ]' : ''
        this.basicExp = json.basic.explains
        this.webExp = json.web
        // for (var i = 0; i < json.web.length; i++) {
        //   this.webExp[i] = web[i]
        // }
        console.log(json)
      }, function (response) {
        // handle error
      })
    }
  }
}
</script>

<style lang="sass">
$theme-color: #02a8fe;
.translax-output {
  display: block;
  padding: 1em .5em;
  background: #f4f4f4;
  font: normal 24px/1.5 "Source Sans Pro", "Helvetica Neue", Arial, sans-serif;
  .basic-output, .web-output {
    margin: 1em 0;
    padding: .5em 3px;
    background: #FFF;
    box-shadow: 0 2px 5px 0 rgba(0,0,0,0.16),0 2px 10px 0 rgba(0,0,0,0.12);
  }
  h2 {
    font: normal bold 21px/1.5 "Source Sans Pro", "Helvetica Neue", Arial, sans-serif;
  }
  h3 {
    position: relative;
    font: normal bold 21px/2 "Source Sans Pro", "Helvetica Neue", Arial, sans-serif;
    &:before {
      content: '§';
      position: absolute;
      top: 3px;
      left: 9px;
      line-height: 1.5;
      color: $theme-color;
    }
  }
  ul li {
    list-style: none;
  }
  p {
    font: normal 18px/1.8 "Source Sans Pro", "Helvetica Neue", Arial, sans-serif;
  }
  .basic-output h3, .basic-output p,
  .web-output h3, .web-output p {
    padding: 0 27px;
  }
  .phonetic-output p {
    padding: 0 9px;
  }
}
</style>
