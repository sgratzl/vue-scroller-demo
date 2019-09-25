<template>
  <table>
    <thead>
      <tr>
        <th></th>
        <th v-for="c in colNames" :key="c">
          {{c}}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(row, i) in cells" :key="i">
        <th>{{rowNames[i]}}</th>
        <td v-for="(v, j) in row" :key="j">
          {{v}}
        </td>
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
    },
    missingPercentage: {
      type: Number,
      default: 0.1
    },
    catColumnPercentage: {
      type: Number,
      default: 0.1
    }
  },

  computed: {
    colNames () {
      return Array.from({ length: this.cols }).map((_, i) => this.toBase26(i))
    },
    rowNames () {
      return Array.from({ length: this.rows }).map((_, i) => (i + 1).toString())
    },
    cells () {
      const cells = []
      const isCategory = this.colNames.map(() => Math.random() < this.catColumnPercentage)
      for (let i = 0; i < this.rows; ++i) {
        const row = []
        for (let j = 0; j < this.cols; ++j) {
          if (row === 0) {
            row.push(this.generateLabel(j))
          } else if (isCategory[j]) {
            row.push(this.generateCategory())
          } else {
            row.push(this.generateValue())
          }
        }
        cells.push(row)
      }
      return cells
    }
  },

  methods: {
    toBase26 (v) {
      let r = []
      let vi = v
      const encode = v => String.fromCharCode('A'.charCodeAt(0) + v)
      while (vi >= 26) {
        r.unshift(encode(Math.floor(vi / 26)))
        vi = vi % 26
      }
      r.unshift(encode(vi))
      return r.join('')
    },
    generateValue () {
      const isMissing = Math.random() < this.missingPercentage
      if (isMissing) {
        return null
      }
      return Math.random().toFixed(3)
    },
    generateLabel (i) {
      return `Col${i}`
    },
    generateCategory () {
      const cats = ['Cat1', 'Cat2', 'Cat3']
      return cats[Math.min(Math.floor(Math.random() * cats.length), cats.length - 1)]
    }
  }
}
</script>

<style scoped>
</style>
