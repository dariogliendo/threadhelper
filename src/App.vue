<template>
  <div id="app">
    <threadText></threadText>
    <contentDivision></contentDivision>
  </div>
</template>

<script>
import threadText from './components/threadText.vue'
import contentDivision from './components/contentDivision.vue'

export default {
  name: 'App',
  components: {
    threadText,
    contentDivision,
  },
}
</script>

<style lang="less">
@import './main.less';
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 800px;
  margin: auto;
}
</style>
