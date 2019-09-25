<template>
  <RecycleScroller class="scroller" :items="columns" :item-size="80" key-field="name" direction="horizontal">
    <template #before>
      <div class="tcr">
        <div class="tch"></div>
        <div v-for="(r,i) in rowNames" :key="r" :class="{trh: true, even: i % 2 ===0, highlight: highlightedRows.has(i)}"
          @click="highlightRow(i)">{{r}}</div>
      </div>
    </template>
    <template #default="{ item, index }">
      <div :class="{tc: true, even: index % 2 ===0, cat: item.isCategory, highlight: highlightedColumns.has(index)}">
        <div :class="{tch: true, even: index % 2 ===0, cat: item.isCategory}"
          @click="highlightColumn(index)">{{item.name}}</div>
        <div v-for="(r,i) in item.values" :key="i" :class="{td: true, even: i % 2 ===0, cat: item.isCategory, highlight: highlightedRows.has(i)}">{{r}}</div>
      </div>
    </template>
  </RecycleScroller>
</template>

<script>
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css'
import { RecycleScroller } from 'vue-virtual-scroller'

export default {
  components: {
    RecycleScroller
  },
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

  data () {
    return {
      highlightedRows: new Set(),
      highlightedColumns: new Set()
    }
  },

  computed: {
    colNames () {
      return Array.from({ length: this.cols }).map((_, i) => base26Converter(i + 1))
    },
    rowNames () {
      return Array.from({ length: this.rows }).map((_, i) => (i + 1).toString())
    },
    columns () {
      const columns = []

      for (let j = 0; j < this.cols; ++j) {
        const isCategory = Math.random() < this.catColumnPercentage
        columns.push({
          isCategory,
          name: this.colNames[j],
          values: isCategory ? this.rowNames.map(() => this.generateCategory()) : this.rowNames.map(() => this.generateValue())
        })
      }
      return columns
    }
  },

  methods: {
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
    },

    highlightColumn (index) {
      if (this.highlightedColumns.has(index)) {
        this.highlightedColumns.delete(index)
      } else {
        this.highlightedColumns.add(index)
      }
      this.highlightedColumns = new Set(this.highlightedColumns)
    },

    highlightRow (index) {
      if (this.highlightedRows.has(index)) {
        this.highlightedRows.delete(index)
      } else {
        this.highlightedRows.add(index)
      }
      this.highlightedRows = new Set(this.highlightedRows)
    }
  }
}

/**
 * Convert decimal to base26 in upper case
 * where A = 1, Z = 26, AA = 27, and 0 cannot be encoded.
 * @param {Number} dec a decimal number to convert
 */
function base26Converter (dec) {
  if (dec < 1) {
    throw new Error('Numbers below 1 cannot be encoded.')
  }
  let str = ''
  let acc = dec
  while (acc > 0) {
    let val = acc % 26
    if (val === 0) {
      val = 26 // Abnormal encoding: 1^26 is 'Z' because there's no 0.
    }
    acc = Math.floor((acc - val) / 26)
    str = String.fromCharCode(64 + val) + str
  }
  return str
}

</script>

<style scoped>

.scroller {
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
}

* {
  box-sizing: border-box;
}

.tc {
  width: 80px;
}

.tch {
  text-align: center;
  border-bottom: 1px solid lightgray;
}

.tcr {
  border-right: 1px solid lightgray;
}

.td {
  text-align: right;
}

.td, .trh, .tch {
  height: 25px;
  padding: 2px 7px;
  white-space: nowrap;
}

.tch {
  background: white;
  position: sticky;
  top: 0;
  z-index: 1;
  cursor: pointer;
}

.trh {
  background: white;
  cursor: pointer;
}

.trh:hover,
.tch:hover {
  color: #1793f8;
}

.td.cat,
.tch.cat {
  background-color: #cd93d8;
  text-align: center;
}

.td:empty {
  background-color: lightblue;
}

.td.highlight,
.trh.highlight {
  background: linear-gradient(180deg,#1793f8,rgba(161,213,255,.4) 2px,rgba(161,213,255,.4) calc(100% - 2px),#1793f8);
}

.tc.highlight {
  background: linear-gradient(90deg,#1793f8,rgba(161,213,255,.4) 2px,rgba(161,213,255,.4) calc(100% - 2px),#1793f8);
}
/*
.tc.even,
.tc.even > .tch,
.trh.even,
.td.even {
  background: rgb(235, 235, 235);
} */

.scroller >>> .vue-recycle-scroller__slot {
  position: sticky;
  left: 0;
  z-index: 1;
}

.scroller >>> .vue-recycle-scroller__item-wrapper {
  overflow: unset;
}

.scroller >>> .vue-recycle-scroller__item-view {
  overflow: unset;
}

</style>
