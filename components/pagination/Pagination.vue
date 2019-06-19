<template>
  <div id='pagination' :class='setting.theme'>
    <div class='pagination-item ctrl-btn prev'
        :style='ctrlWidth'
        @click='changePage("prev")'></div>

    <div class='pagination-item'
          :class='setting.current === 1 && "current"'
          :style='itemWidth'
          v-if='setting.boundary'
          @click='changePage(1)'>
            <p>1</p>
          </div>

    <div class='pagination-item'
          v-for='(item, index) in new Array(setting.length)'
          :style='itemWidth'
          :key='index'
          :class='firstNum + index === setting.current && "current"'
          @click='changePage($event.target.innerText)'>
            <p v-text='filterText(index)'></p>
          </div>

    <div class='pagination-item'
          :class='setting.current === setting.last && "current"'
          :style='itemWidth'
          v-if='setting.boundary'
          @click='changePage(setting.last)'>
            <p v-text='setting.last'></p>
          </div>

    <div class='pagination-item ctrl-btn next'
          :style='ctrlWidth'
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
          typeof value.current === 'number' &&
          ['boolean', 'undefined'].indexOf(typeof value.boundary) !== -1 &&
          ['string', 'undefined'].indexOf(typeof value.theme) !== -1 &&
          value.length <= value.last &&
          value.current <= value.last
        )
      }
    }
  },
  data: function () {
    return {
    }
  },
  methods: {
    changePage (page) {
      if (page === 'prev' && this.setting.current > 1) {
        this.$emit('current-page', this.setting.current - 1)
      } else if (page === 'next' && this.setting.current < this.setting.last) {
        this.$emit('current-page', this.setting.current + 1)
      } else if (!isNaN(Number(page))) {
        this.$emit('current-page', Number(page))
      }
    },
    filterText (index) {
      if (this.setting.boundary) {
        const start = index === 0 && this.setting.current > this.middle + 1
        const end = index === this.setting.length - 1 && this.setting.current < this.setting.last - this.middle
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
      if (this.setting.current <= this.middle) {
        return this.setting.boundary ? 2 : 1
      } else if (this.setting.current >= this.setting.last - this.middle + 1) {
        return this.setting.last - this.setting.length + (this.setting.boundary ? 0 : 1)
      } else {
        return this.setting.current - this.middle + 1
      }
    },
    itemWidth () {
      return {minWidth: 100 / (this.setting.length + 4) + '%'}
    },
    ctrlWidth () {
      let condition = ['blunt'].indexOf(this.setting.theme) !== -1
      return {
        minWidth: 100 / (this.setting.length + 4) + (condition ? 2 : 0) + '%'
      }
    }
  }
}
</script>

<style>
@import './style/texture-dark.css';
@import './style/bubble-ching.css';
@import './style/blunt.css';

#pagination {
  display: flex;
  justify-content: space-between;
}

.pagination-item {
  font-size: 0;
  cursor: pointer;
  user-select: none;
  box-sizing: border-box;
  position: relative;
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

.ctrl-btn::after {
  content: '';
  display: inline-block;
  width: 20%;
  height: 20%;
  border-top: 2px solid #bebebf;
  border-right: 2px solid transparent;
  border-bottom: 2px solid transparent;
  border-left: 2px solid #bebebf;
  position: absolute;
  top: 50%;
  left: 50%;
}

.pagination-item::before {
  content: '';
  display: inline-block;
  padding: 100% 0 0 0;
}

.prev::after {
  transform: translate(calc(100% * 1.414 / 4 - 50%), -50%) rotate(-45deg);
}

.next::after {
  transform: translate(calc(-100% * 1.414 / 4 - 50%), -50%) rotate(135deg);
}
</style>
