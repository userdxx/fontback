<template>
  <div class="card">
    <div class="cardHeader">
      <slot name="slotHeader" />
    </div>
    <div class="cardBody">
      <slot name="slotBody" />
    </div>
    <div class="cardFooter">
      <slot name="slotFooter" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {}
  }
}
</script>

<style lang="scss" scoped>
.card {
  height: 100%;
  background: white;
  .cardHeader {
    height: 49px;
    border-bottom: 1px solid #eee;
  }
  .cardBody {
    height: calc(100% - 100px);
    min-height: 120px;
  }
  .cardFooter {
    height: 49px;
    border-top: 1px solid #eee;
  }
}
</style>
