<template>
  <div id="app">
    <virtual-table :cols="cols" :rows="rows"/>
  </div>
</template>

<script>
import VirtualTable from './components/VirtualTable.vue'

export default {
  name: 'app',
  components: {
    VirtualTable
  },
  data () {
    const url = new URL(window.location.href)
    return {
      cols: parseInt(url.searchParams.get('cols') || 10, 10),
      rows: parseInt(url.searchParams.get('cols') || 10, 10)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
