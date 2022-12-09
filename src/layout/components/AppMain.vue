<template>
  <section class="app-main">
    <!-- 面包屑 -->
    <div class="breadcrumb">
      <breadcrumb class="breadcrumb-container" />
    </div>
    <transition name="fade-transform" mode="out-in">
      <router-view :key="key" />
    </transition>
  </section>
</template>

<script>
import Breadcrumb from '@/components/Breadcrumb'

export default {
  name: 'AppMain',
  components: {
    Breadcrumb
  },
  computed: {
    key() {
      return this.$route.path
    }
  }
}
</script>

<style scoped>
.app-main {
  /*50 = navbar  */
  min-height: calc(100vh - 50px);
  width: 100%;
  position: relative;
  overflow: hidden;
  border-left: 2px solid #f1f2f3;
}
.fixed-header+.app-main {
  padding-top: 50px;
}
</style>

<style lang="scss">
// fix css style bug in open el-dialog
.el-popup-parent--hidden {
  .fixed-header {
    padding-right: 15px;
  }
}
  .breadcrumb {
    border-bottom: 2px solid #f1f2f3;
  }

  .breadcrumb-container {
    height: 30px !important;
    line-height: 38px !important;
    box-sizing: border-box;
  }
</style>
