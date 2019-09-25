<template>
  <table>
    <thead>
      <tr>
        <th v-for="c in colNames" :key="c">
          {{c}}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(row, i) in cells" :key="i">
        <th v-for="(v, j) in row" :key="j">
          {{v}}
        </th>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    cols: {
      type: Number,
      required: true
    },
    rows: {
      type: Number,
      required: true
    }
  },

  computed: {
    colNames () {
      return Array.from({ length: this.cols }).map((_, i) => i.toString())
    },
    rowNames () {
      return Array.from({ length: this.rows }).map((_, i) => i.toString())
    },
    cells () {
      const cells = []
      for (let i = 0; i < this.rows; ++i) {
        const row = []
        for (let j = 0; j < this.cols; ++j) {
          row.push(Math.random())
        }
        cells.push(row)
      }
      return cells
    }
  }
}
</script>

<style scoped>
</style>
