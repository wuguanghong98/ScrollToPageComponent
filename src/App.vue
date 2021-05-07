<template>
  <div id="app">
    <!--调用使用方法-->
    <scroll-to-page arrowColor="red" liBackground="#BD362F" liOnColor="#000" :btnNum="4">
      <div slot="1">
        <span>
          我是内容一
        </span>
        <hr>
      </div>
      <div slot="2">
        <p>
          我是内容二
        </p>
        <hr>
      </div>
      <div slot="3">
        <h1>
          我是内容三
        </h1>
        <hr>
      </div>
      <div slot="4">
        <h1>
          我是内容四
        </h1>
        <hr>
      </div>
    </scroll-to-page>
  </div>
</template>

<script>
  import ScrollToPage from './components/ScrollToPage'

export default {
  name: 'App',
  components: {
    ScrollToPage
  },
  data() {
    return {
    }
  }

}
</script>

<style lang="scss">

  body {
    margin:0;
    padding:0;
  }
  html,body{
    width:100%;
    height:100%;
    overflow:hidden;
  }
  #app {
    height: 100%;
    width: 100%;
  }
</style>
