<template>
  <div id="app">
    <keep-alive exclude="detail">
      <router-view></router-view>
      </keep-alive>
    <Maintabbar></Maintabbar>
        <icon></icon>
    <svg-icon></svg-icon>
  </div>
</template>

<script>

  import Maintabbar from './components/content/maintabbar.vue'
  import Icon from './components/content/Icon/Icon.vue'
  import SvgIcon from './components/content/Icon/svg.vue'
  export default {
    name: 'App',
    components: {
      Maintabbar,
      Icon,
      SvgIcon
    }
  }
</script>

<style>
  @import "./assets/css/base.css";
</style>
