<template>
  <div class='waterfall'>
    <div class='waterfall-group'
         v-for='( group, index ) in groups'
         :class='"group" + index'
         :key='index'
         :ref='"group" + index'
         :style='{width: setting.colWidth}'>
      <div class='waterfall-cell'
           v-for='(cell, cellIndex) in groups[index]["images"]'
           :key='cellIndex'
           :style='{marginBottom: setting.margin.replace("px", "") / groupsEl[index][0].offsetWidth * 100 + "%"}'>
        <img :src='cell'
             alt=''>
        <slot name='cell'
              :url='cell'
              :index='cellIndex'>
        </slot>
      </div>
      <div class='filled'
           :style='{height: 0, paddingTop: groups[index].filled + "%", backgroundColor: setting.fill}'
           v-if='groups[index].filled && images.length === 0'></div>
      <slot name='group'
            :group='group'
            :index='index'></slot>
    </div>
    <slot name='whole'></slot>
    <div v-if='setting.loadingImg'
         v-show='loading'
         class='loading'>
      <img :src="loadingImg"
           alt="">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Waterfall',
  props: {
    setting: {
      type: Object,
      validator: function (value) {
        return (
          typeof Number(value.colNum) === 'number' &&
          typeof value.colWidth === 'string' &&
          ['string', 'undefined'].indexOf(typeof value.margin) !== -1 &&
          ['string', 'undefined'].indexOf(typeof value.fill) !== -1 &&
          ['string', 'undefined'].indexOf(typeof value.failImg) !== -1 &&
          ['string', 'undefined'].indexOf(typeof value.loadingImg) !== -1
        )
      }
    },
    urls: {
      type: Array,
      default () {
        return []
      }
    }
  },
  data: function () {
    return {
      min: 0,
      images: [],
      groups: new Array(this.setting.colNum).fill({ filled: false, images: [] }),
      groupsEl: [],
      loadingImg: this.setting.loadingImg || '',
      failImg: this.setting.failImg || '',
      loading: false
    }
  },
  methods: {
    getGroupsEl () {
      for (let i in this.$refs) {
        this.groupsEl.push(this.$refs[i])
      }
    },
    preload (diff) {
      /* 加载图片 */
      const loader = diff.map((item, index) => {
        return new Promise((resolve, reject) => {
          let image = new Image()
          image.src = item
          image.onload = () => resolve()
          image.onerror = () => resolve(index)
        })
      })
      return Promise.all(loader)
    },
    trim (diff, loaded) {
      // 替换无效图片
      if (this.setting.failImg) {
        return diff.map((item, index) => {
          if (loaded.indexOf(index) !== -1) {
            item = this.setting.failImg
          }
          return item
        })
      }
      // 筛除无效图片
      return diff.filter((item, index) => {
        return loaded.indexOf(index) === -1
      })
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
          this.groupsEl[index][0].offsetHeight - this.setting.margin.replace('px', '')
        height = height / this.groupsEl[index][0].offsetWidth * 100
        return Object.assign(item, { filled: height })
      })
    },
    loadState () {
      /* 加载状态 */
      this.$emit('load-state', this.loading)
    }
  },
  mounted () {
    this.getGroupsEl()
    this.preload([this.failImg, this.loadingImg])
  },
  watch: {
    urls (now, before) {
      this.loading = true
      let diff = now.filter((item, index) => {
        return !(index in before)
      })
      this.preload(diff)
        .then(loaded => {
          let useable = this.trim(diff, loaded)
          if (useable.length === 0) {
            this.loading = false
            return
          }
          this.images = useable
        })
    },
    images (val) {
      this.$nextTick(() => {
        if (this.images.length === 0) {
          if (this.setting.fill) this.fill()
          this.loading = false
          return
        }
        let newValue = {
          filled: false,
          images: [
            ...this.groups[this.min]['images'],
            this.images.shift()]
        }
        this.$set(this.groups, this.min, newValue)
      })
    },
    loading () {
      this.loadState()
    }
  },
  updated () {
    this.getMin()
  }
}
</script>

<style scoped>
.waterfall {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  position: relative;
}

.waterfall-group {
  align-self: flex-start;
  position: relative;
}

.waterfall-cell {
  position: relative;
  font-size: 0;
}

.waterfall-cell > * {
  font-size: initial;
}

.waterfall-cell img {
  width: 100%;
  height: auto;
}

.waterfall-group .filled {
  width: 100%;
  background-color: red;
}

.loading {
  margin: 0 auto;
}
</style>
