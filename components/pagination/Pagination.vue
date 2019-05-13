<template>
  <div id='pagination' :class='setting.theme'>
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
          ['string', 'undefined'].indexOf(typeof value.theme) !== -1 &&
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
@import './style/texture-dark.css';
@import './style/bubble-ching.css';

#pagination {
  display: flex;
  justify-content: space-between;
}
</style>
