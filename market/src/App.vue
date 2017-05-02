<template>
  <div id="view">
    <v-header></v-header>
    <v-tab></v-tab>
  </div>
</template>

<script>
  import header from './components/header/header.vue';
  import tab from './components/tab/tab.vue';
  export default{
      components:{
        "v-header":header,
        "v-tab":tab
      }
  }
</script>

<style>
  @import "common/stylus/base.styl";
</style>
