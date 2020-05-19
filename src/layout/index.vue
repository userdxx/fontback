<template>
  <div :class="classObj">
    <div v-if="device==='mobile'&&sidebar.opened" class="drawer-bg" @click="handleClickOutside" />
    <sidebar class="sidebar-container" />
    <div :class="{hasTagsView:needTagsView}" class="main-container">
      <div :class="{'fixed-header':fixedHeader}">
        <navbar />
        <tags-view v-if="needTagsView" />
      </div>
      <app-main />
      <right-panel v-if="showSettings">
        <settings />
      </right-panel>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import RightPanel from '@/components/RightPanel'
import { AppMain, Navbar, Settings, Sidebar, TagsView } from './components'
import ResizeMixin from './mixin/ResizeHandler'
import { mapState } from 'vuex'

export default {
  name: 'Layout',
  components: {
    AppMain,
    Navbar,
    RightPanel,
    Settings,
    Sidebar,
    TagsView
  },
  mixins: [ResizeMixin],
  computed: {
    ...mapState({
      sidebar: state => state.app.sidebar,
      device: state => state.app.device,
      showSettings: state => state.settings.showSettings,
      needTagsView: state => state.settings.tagsView,
      fixedHeader: state => state.settings.fixedHeader
    }),
    classObj() {
      return {
        hideSidebar: !this.sidebar.opened,
        openSidebar: this.sidebar.opened,
        withoutAnimation: this.sidebar.withoutAnimation,
        mobile: this.device === 'mobile'
      }
    }
  },
  created() {
    const live2d_path = '../live2d/'
    Promise.all([
      this.loadExternalResource(
        live2d_path + 'waifu.css?v=' + new Date().valueOf(),
        'css'
      ),
      this.loadExternalResource(
        live2d_path + 'live2d.min.js?v=' + new Date().valueOf(),
        'js'
      ),
      this.loadExternalResource(
        live2d_path + 'waifu-tips.js?v=' + new Date().valueOf(),
        'js'
      )
    ]).then(() => {
      initWidget({
        waifuPath: live2d_path + 'waifu-tips.json',
        apiPath: 'https://live2d.fghrsh.net/api/'
        //  cdnPath: 'https://cdn.jsdelivr.net/gh/fghrsh/live2d_api/'
      })
    })
  },
  methods: {
    loadExternalResource(url, type) {
      return new Promise((resolve, reject) => {
        let tag

        if (type === 'css') {
          tag = document.createElement('link')
          tag.rel = 'stylesheet'
          tag.href = url
        } else if (type === 'js') {
          tag = document.createElement('script')
          tag.src = url
        }
        if (tag) {
          tag.onload = () => resolve(url)
          tag.onerror = () => reject(url)
          document.head.appendChild(tag)
        }
      })
    },
    handleClickOutside() {
      this.$store.dispatch('app/closeSideBar', { withoutAnimation: false })
    }
  }
}
</script>

<style lang='scss' scoped>
@import '~@/styles/mixin.scss';
@import '~@/styles/variables.scss';
@import url("https://cdn.jsdelivr.net/npm/font-awesome/css/font-awesome.min.css");
.app-wrapper {
  @include clearfix;
  position: relative;
  height: 100%;
  width: 100%;

  &.mobile.openSidebar {
    position: fixed;
    top: 0;
  }
}

.drawer-bg {
  background: #000;
  opacity: 0.3;
  width: 100%;
  top: 0;
  height: 100%;
  position: absolute;
  z-index: 999;
}

.fixed-header {
  position: fixed;
  top: 0;
  right: 0;
  z-index: 9;
  width: calc(100% - #{$sideBarWidth});
  transition: width 0.28s;
}

.hideSidebar .fixed-header {
  width: calc(100% - 54px);
}

.mobile .fixed-header {
  width: 100%;
}
</style>
