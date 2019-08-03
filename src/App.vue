<template>
  <div id="app">
    <YList msg="Видео с Youtube по запросу"/>
  </div>
</template>

<script>
import YList from './components/YList.vue'

export default {
  name: 'app',
  components: {
    YList
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 30px;
}

</style>
