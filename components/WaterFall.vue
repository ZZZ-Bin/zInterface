<template>
  <div id="waterfall">
    <div class="waterfall-group"
      v-for='( group, index ) in groups'
      :class='"group" + index'
      :key='index + 1'
      :ref='"group" + index'
      :style='{width: settings.colWidth}'>
      <div class="waterfall-cell"
        v-for='(cell, cellIndex) in groups[index]["images"]'
        :key='cellIndex'
        :style='{marginBottom: settings.margin / groupsEl[index][0].offsetWidth * 100 + "%"}'>
        <img :src="cell" alt="">
      </div>
      <div class='over'
      :style='{height: 0, paddingTop: groups[index].filled + "%", backgroundColor: settings.fill}'
      v-if='groups[index].filled && images.length === 0'></div>
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
      fill: { default: false }
    },
    datas: {
      type: Array
    }
  },
  data: function () {
    return {
      min: 0,
      images: [],
      groups: new Array(this.settings.colNum).fill({filled: false, images: []}),
      groupsEl: []
    }
  },
  methods: {
    getGroupsEl () {
      for (let i in this.$refs) {
        this.groupsEl.push(this.$refs[i])
      }
    },
    getMin () {
      /* 视图更新后获取高度最小的 group，更新 this.min */
      this.min = [...this.groupsEl].sort((a, b) => {
        return a[0].offsetHeight - b[0].offsetHeight
      })[0][0].classList[1].replace('group', '')
    },
    fill () {
      /* 自动填充 */
      const max = [...this.groupsEl].sort((a, b) => {
        return b[0].offsetHeight - a[0].offsetHeight
      })[0][0].classList[1].replace('group', '')
      this.groups = this.groups.map((item, index) => {
        let height = this.groupsEl[max][0].offsetHeight -
          this.groupsEl[index][0].offsetHeight - this.settings.margin
        height = height / this.groupsEl[index][0].offsetWidth * 100
        return Object.assign(item, {filled: height})
      })
    }
  },
  mounted () {
    this.getGroupsEl()
  },
  watch: {
    datas (now, before) {
      let diff = now.filter((item, index) => {
        return !(index in before)
      })
      let preload = diff.map((item, index) => {
        let image = new Image()
        image.src = item
        return new Promise((resolve, reject) => {
          image.onload = () => resolve()
          image.onerror = () => resolve(index)
        })
      })
      Promise.all(preload).then(loaded => {
        let useable = diff.filter((item, index) => {
          return loaded.indexOf(index) === -1
        })
        if (useable.length === 0) return
        this.images = useable
      })
    },
    images (val, before) {
      this.$nextTick(() => {
        if (this.images.length === 0) {
          if (this.settings.fill) this.fill()
          return
        }
        this.$set(this.groups,
          this.min,
          {filled: false,
            images: [
              ...this.groups[this.min]['images'],
              this.images.shift() ]})
      })
    }
  },
  updated () {
    this.getMin()
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

.waterfall-group .over {
  width: 100%;
  background-color: red;
}

</style>
