<template>
  <div id="waterfall">
    <div class="waterfall-group"
      v-for='( group, index ) in groups'
      :class='"group" + index'
      :key='index + 1'
      :ref='"group" + index'
      :style='"width:" + settings.colWidth + "px"'>
      <div class="waterfall-cell"
        v-for='(cell, cellIndex) in groups[index]["images"]'
        :key='cellIndex'
        :style='{marginBottom: settings.margin + "px"}'>
        <img :src="cell" alt="">
      </div>
      <div class='over'
      :style='{height: groups[index].isOver + "px", backgroundColor: settings.fill}'
      v-if='groups[index].isOver && images.length === 0'></div>
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
      images: this.datas,
      groups: new Array(this.settings.colNum).fill({isOver: false, images: []}),
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
    useable (image, length) {
      /* 图片加载成功 */
      this.$set(this.groups,
        this.min,
        {isOver: false,
          images: [
            ...this.groups[this.min]['images'],
            image.src ]})
    },
    unUesable () {
      /* 图片加载失败 */
    },
    fill () {
      /* 自动填充 */
      this.$nextTick(() => {
        const max = [...this.groupsEl].sort((a, b) => {
          return b[0].offsetHeight - a[0].offsetHeight
        })[0][0].classList[1].replace('group', '')
        this.groups = this.groups.map((item, index) => {
          let height = this.groupsEl[max][0].offsetHeight -
            this.groupsEl[index][0].offsetHeight - this.settings.margin
          return Object.assign(item, {isOver: height})
        })
      })
    }
  },
  mounted () {
    this.getGroupsEl()
  },
  watch: {
    images () {
      this.$nextTick(() => {
        if (this.images.length === 0) return
        const image = new Image()
        image.src = this.images[0]
        new Promise((resolve, reject) => {
          const length = this.images.length
          image.onload = () => {
            resolve([length, true])
          }
          image.onerror = () => {
            resolve([length, false])
          }
        }).then(([length, canUse]) => {
          this.images.shift()
          if (canUse) this.useable(image, length)
          else this.unUesable(image, length)
          if (this.images.length === 0) {
            this.fill()
          }
        }).catch(err => {
          throw err
        })
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
