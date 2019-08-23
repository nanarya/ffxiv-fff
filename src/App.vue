<template lang="pug">
  <div id="app">
    h1.pageTitle FINAL FANTASY XIV Friend's Friend Finder
    HelloWorld(msg="Welcome to Your Vue.js App")
    hr
    .info
      p
        |このアプリケーションを使うには、調べたいキャラクターを両方ともLODESTONEにて、フレンドを公開設定にしている必要があります。
        |ログインの上、<a target="_blank" href="https://jp.finalfantasyxiv.com/lodestone/my/setting/account/">アカウント設定</a>を確認してください。
      p
        |記載されている会社名・製品名・システム名などは、各社の商標、または登録商標です。
        br
        |Copyright (C) 2010 - 2019 SQUARE ENIX CO., LTD. All Rights Reserved.

      p
        |ご意見ご要望は<a target="_blank" href="https://twitter.com/kid_nanarya">@kid_nanarya</a>まで

      p
        |ハッシュタグは<a target="_blank" href="https://twitter.com/search?q=%23FF14FFF&src=typed_query#FF14FFF">こちら</a>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style lang="stylus">
.pageTitle
  font-size 30px
  margin-bottom 50px
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width 90%
  max-width 960px
  margin 0 auto
  padding 20px 0
}
.info
  font-size 10px
  line-height 1.5
  text-align left
/* 
html5doctor.com Reset Stylesheet
v1.6.1
Last Updated: 2010-09-17
Author: Richard Clark - http://richclarkdesign.com 
Twitter: @rich_clark
*/

html, body, div, span, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
abbr, address, cite, code,
del, dfn, em, img, ins, kbd, q, samp,
small, strong, sub, sup, var,
b, i,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section, summary,
time, mark, audio, video {
    margin:0;
    padding:0;
    border:0;
    outline:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

body {
    line-height:1;
}

article,aside,details,figcaption,figure,
footer,header,hgroup,menu,nav,section { 
    display:block;
}

nav ul {
    list-style:none;
}

blockquote, q {
    quotes:none;
}

blockquote:before, blockquote:after,
q:before, q:after {
    content:'';
    content:none;
}

a {
    margin:0;
    padding:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

/* change colours to suit your needs */
ins {
    background-color:#ff9;
    color:#000;
    text-decoration:none;
}

/* change colours to suit your needs */
mark {
    background-color:#ff9;
    color:#000; 
    font-style:italic;
    font-weight:bold;
}

del {
    text-decoration: line-through;
}

abbr[title], dfn[title] {
    border-bottom:1px dotted;
    cursor:help;
}

table {
    border-collapse:collapse;
    border-spacing:0;
}

/* change border colour to suit your needs */
hr {
    display:block;
    height:1px;
    border:0;   
    border-top:1px solid #cccccc;
    margin:1em 0;
    padding:0;
}

input, select {
    vertical-align:middle;
}
</style>
