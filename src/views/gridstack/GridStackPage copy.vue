<template>
  <div>
    <div class="grid-stack" data-gs-animate="yes">
      <div class="grid-stack-item" data-gs-x="0" data-gs-y="0" data-gs-width="4" data-gs-height="2">
        <div class="grid-stack-item-content">1</div>
      </div>
      <div class="grid-stack-item" data-gs-x="4" data-gs-y="0" data-gs-width="4" data-gs-height="4">
        <div class="grid-stack-item-content">
          不能移动和缩放！
          <br />
        </div>
      </div>
      <div
        class="grid-stack-item"
        data-gs-x="8"
        data-gs-y="0"
        data-gs-width="2"
        data-gs-height="2"
        data-gs-min-width="2"
        data-gs-no-resize="yes"
      >
        <div class="grid-stack-item-content" style="overflow: hidden">
          <p class="card-text text-center" style="margin-bottom: 0">Drag me!</p>
          <p class="card-text text-center" style="margin-bottom: 0"></p>
          <p class="card-text text-center" style="margin-bottom: 0">不能缩放</p>
        </div>
      </div>
      <div
        class="grid-stack-item"
        data-gs-x="10"
        data-gs-y="0"
        data-gs-width="2"
        data-gs-height="2"
      >
        <div class="grid-stack-item-content">4</div>
      </div>
      <div class="grid-stack-item" data-gs-x="0" data-gs-y="2" data-gs-width="2" data-gs-height="2">
        <div class="grid-stack-item-content">5</div>
      </div>
      <div class="grid-stack-item" data-gs-x="2" data-gs-y="2" data-gs-width="2" data-gs-height="4">
        <div class="grid-stack-item-content">6</div>
      </div>
      <div class="grid-stack-item" data-gs-x="8" data-gs-y="2" data-gs-width="4" data-gs-height="2">
        <div class="grid-stack-item-content">7</div>
      </div>
      <div class="grid-stack-item" data-gs-x="0" data-gs-y="4" data-gs-width="2" data-gs-height="2">
        <div class="grid-stack-item-content">8</div>
      </div>
      <div class="grid-stack-item" data-gs-x="4" data-gs-y="4" data-gs-width="4" data-gs-height="2">
        <div class="grid-stack-item-content">9</div>
      </div>
      <div class="grid-stack-item" data-gs-x="8" data-gs-y="4" data-gs-width="2" data-gs-height="2">
        <div class="grid-stack-item-content">10</div>
      </div>
      <div
        class="grid-stack-item"
        data-gs-x="10"
        data-gs-y="4"
        data-gs-width="2"
        data-gs-height="2"
      >
        <div class="grid-stack-item-content" @click="maxSize(10)">11</div>
      </div>
    </div>
  </div>
</template>
<script type="text/javascript">
/* eslint-disable */

export default {
  name: 'grid_stack_page',
  data() {
    return {
      modalVisible: false,
      grid: {}
    }
  },
  computed: {
    content() {
      return this.$t('content')
    }
  },
  mounted() {
    this.grid = GridStack.init()
  },
  activated() {
    this.grid = GridStack.init()
    this.grid.addWidget('', { width: 2 })
  },
  methods: {
    maxSize(e) {
      console.log(this.grid)
      this.grid.update(this.grid.el.children[e], 0, 0, 12, 12)
    }
  }
}
</script>
<style scoped>
@import url(grid_stack_page.css);
</style>
