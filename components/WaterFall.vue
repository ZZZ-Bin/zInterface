<template>
  <div id="waterfall">
    <div class="waterfall-group"
      v-for='( group, index ) in groups'
      :class='"group" + index'
      :key='index + 1'
      :ref='"group" + index'
      :style='"width:" + settings.colWidth + "px"'>
      <div class="waterfall-cell"
        v-for='(cell, cellIndex) in groups[index]'
        :key='cellIndex'>
        <img :src="cell.url" alt="">
        <p>{{ cell.id }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WaterFall',
  props: {
    settings: {
      type: Object,
      direction: String,
      colNum: Number,
      colWidth: Object,
      completed: Boolean
    },
    datas: {
      type: Array
    }
  },
  data: function () {
    return {
      min: 0,
      images: this.datas,
      groups: new Array(this.settings.colNum).fill([]),
      groupsEl: []
    }
  },
  mounted () {
    for (let i in this.$refs) {
      this.groupsEl.push(this.$refs[i])
    }
  },
  watch: {
    images () {
      if (this.images.length === 0) return
      this.$nextTick(() => {
        this.$set(this.groups,
          this.min,
          [ ...this.groups[this.min], this.images.shift() ])
      })
    }
  },
  updated () {
    this.groupsEl.sort((a, b) => {
      return a[0].offsetHeight - b[0].offsetHeight
    })
    this.min = this.groupsEl[0][0].classList[1].replace('group', '')
  }
}
</script>

<style>
#waterfall {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.waterfall-group {
  align-self: flex-start;
  font-size: 0;
}

.waterfall-cell img {
  width: 100%;
  height: auto;
}

.waterfall-cell p {
  font-size: 12px;
}

</style>
