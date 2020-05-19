<template>
  <demo-block>
    <template slot="preview">
      <model-gltf
        :backgroundAlpha="0"
        @on-load="onLoad"
        src="../../static/models/gltf/Duck/glTF/Duck.gltf"
      ></model-gltf>
      <div class="example-loading" v-show="loading"></div>
    </template>
  </demo-block>
</template>

<script>
/* eslint-disable */
import DemoBlock from "@/components/demo-block.vue";
import ModelGltf from "./model/model-gltf.vue"; 

export default {
  name: "demo-gltf",
  data() {
    return { 
      loading: true
    };
  },
  methods: {
    onLoad() {
      this.loading = false;
    }
  },
  components: {
    ModelGltf,
    DemoBlock
  }
};
</script>
