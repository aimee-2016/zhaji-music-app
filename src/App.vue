<template>
  <div id="app">
    <m-header></m-header>
    <tab></tab>
    <router-view/>
  </div>
</template>

<script>
import MHeader from './components/m-header/m-header'
import tab from './components/tab/tab'
export default {
  name: 'App',
  components: {
    tab,
    MHeader
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">

</style>
