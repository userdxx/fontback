<template>
  <div class="demo-block">
    <div class="demo-block-preview">
      <slot name="preview"></slot>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: "app",
  data() {
    return {};
  },
  components: {}
};
</script>
<style>
.demo-block {
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0);
  border-radius: 3px;
}

.demo-block .demo-block-preview { 
  width: 80%;
  height: 100%;
  float: left;
  position: relative; 
} 
 
 
</style>
