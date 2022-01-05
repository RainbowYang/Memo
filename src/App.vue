<template>
  <div id="app">
    <Header/>
    <Content/>
    <Footer/>
  </div>
</template>

<script>
import Header from '@/components/Header'
import Footer from '@/components/Footer'
import Content from '@/components/Content'

export default {
  name: 'app',
  components: { Header, Footer, Content },
}
</script>

<style>
body {
  margin: 0;
}

#app {
  padding: 16px 0;
  box-sizing: border-box;
  height: 100vh;
  display: flex;
  flex-direction: column;

  background-color: #FFF8F3;
  justify-items: center;
}

* {
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
}
</style>
