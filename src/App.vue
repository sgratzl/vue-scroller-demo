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
      cols: parseInt(url.searchParams.get('cols') || 2000, 10),
      rows: parseInt(url.searchParams.get('rows') || 200, 10)
    }
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Barlow Condensed,sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: relative;
  font-size: 14px;
  line-height: 1.5;
  width: 100vw;
  height: 100vh;
}
</style>
