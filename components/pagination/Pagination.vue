<template>
  <div id='pagination'>
    <div class='pagination-item ctrl-btn prev'
        :style='itemWidth'
        @click='changePage("prev")'></div>

    <div class='pagination-item'
          :class='currentPage === 1 && "current"'
          :style='itemWidth'
          v-if='setting.boundary'
          @click='changePage(1)'>
            <p>1</p>
          </div>

    <div class='pagination-item'
          v-for='(item, index) in new Array(setting.length)'
          :style='itemWidth'
          :key='index'
          :class='firstNum + index === currentPage && "current"'
          @click='changePage($event.target.innerText)'>
            <p v-text='filterText(index)'></p>
          </div>

    <div class='pagination-item'
          :class='currentPage === setting.last && "current"'
          :style='itemWidth'
          v-if='setting.boundary'
          @click='changePage(setting.last)'>
            <p v-text='setting.last'></p>
          </div>

    <div class='pagination-item ctrl-btn next'
          :style='itemWidth'
          @click='changePage("next")'></div>
  </div>
</template>

<script>
export default {
  name: 'Pagination',
  props: {
    setting: {
      type: Object,
      validator: function (value) {
        return (
          typeof value.length === 'number' &&
          typeof value.last === 'number' &&
          ['boolean', 'undefined'].indexOf(typeof value.boundary) !== -1 &&
          ['number', 'undefined'].indexOf(typeof value.current) !== -1 &&
          value.length <= value.last &&
          value.current <= value.last
        )
      }
    }
  },
  data: function () {
    return {
      currentPage: this.setting.current || 1
    }
  },
  methods: {
    changePage (page) {
      if (page === 'prev' && this.currentPage > 1) {
        this.currentPage -= 1
      } else if (page === 'next' && this.currentPage < this.setting.last) {
        this.currentPage += 1
      } else if (!isNaN(Number(page))) {
        this.currentPage = Number(page)
      }
    },
    filterText (index) {
      if (this.setting.boundary) {
        const start = index === 0 && this.currentPage > this.middle + 1
        const end = index === this.setting.length - 1 && this.currentPage < this.setting.last - this.middle
        if (start || end) return '...'
        else return this.firstNum + index
      } else {
        return this.firstNum + index
      }
    }
  },
  computed: {
    middle () {
      return Math.ceil(this.setting.length / 2)
    },
    firstNum () {
      if (this.currentPage <= this.middle) {
        return this.setting.boundary ? 2 : 1
      } else if (this.currentPage >= this.setting.last - this.middle + 1) {
        return this.setting.last - this.setting.length + (this.setting.boundary ? 0 : 1)
      } else {
        return this.currentPage - this.middle + 1
      }
    },
    itemWidth () {
      return {width: 100 / (this.setting.length + 4) + '%'}
    }
  },
  watch: {
    currentPage () {
      this.$emit('current-page', this.currentPage)
    },
    setting: {
      deep: true,
      handler () {
        this.currentPage = this.setting.current
      }
    }
  }
}
</script>

<style>
#pagination {
  display: flex;
  justify-content: space-between;
}
/* texture-dark */
/* .pagination-item {
  border-radius: 15%;
  background: linear-gradient(#b8b8b8, #3f3f3f 30%, #444 60%, #111);
  color: #fff;
  font-size: 0;
  cursor: pointer;
  user-select: none;
  box-sizing: border-box;
  box-shadow: 1.5px 1.5px 3px rgb(117, 96, 1), 0 1.5px 2px rgb(192, 192, 192) inset;
  border-top: 1px solid rgb(230, 230, 230);
  border-right: 1px solid rgb(32, 32, 32);
  border-bottom: 1px solid rgb(32, 32, 32);
  border-left: 1px solid rgb(225, 225, 225);
  position: relative;
}

.pagination-item::before {
  content: '';
  display: inline-block;
  padding: 100% 0 0 0;
}

.pagination-item p {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  margin: 0;
  padding: 0;
  font-size: initial;
}

.pagination-item:hover {
  background: linear-gradient(#f1decf, #ef833a 30%, #ce7132 60%, #583115);
  border-right: 1px solid #924f23;
  border-bottom: 1px solid #5c3216;
}

.ctrl-btn::after {
  content: '';
  display: inline-block;
  width: 20%;
  height: 20%;
  border-top: 2px solid #fff;
  border-right: 2px solid transparent;
  border-bottom: 2px solid transparent;
  border-left: 2px solid #fff;
  position: absolute;
  top: 50%;
  left: 50%;
}

.prev::after {
  transform: translate(calc(100% * 1.414 / 4 - 50%), -50%) rotate(-45deg);
}

.next::after {
  transform: translate(calc(-100% * 1.414 / 4 - 50%), -50%) rotate(135deg);
}

.current {
  background: linear-gradient(#f1decf, #ef833a 30%, #ce7132 60%, #583115);
  border-right: 1px solid #924f23;
  border-bottom: 1px solid #5c3216;
} */
</style>
