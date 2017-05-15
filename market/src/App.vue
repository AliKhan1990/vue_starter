<template>
  <div id="view">
    <v-intro v-on:enter="closeIntro" v-show="!intro"></v-intro>
    <v-header v-show="intro" :seller="seller"></v-header>
    <v-tab v-show="intro" :seller="seller"></v-tab>
  </div>
</template>

<script>
  import header from './components/header/header';
  import tab from './components/tab/tab';
  import intro from './components/introduction/introduction'
  const ERR_OK = 0;
  export default{
    data(){
      return {
        seller: {},
        headerHeight:0,
        intro:true
      }
    },
    created(){
      this.axios.get("/api/seller").then(res => {
        res = res.data;
        if (res.errno == ERR_OK) {
          this.seller = res.data;
        }
      });
    },
    mounted(){

    },
    components: {
      "v-header": header,
      "v-tab": tab,
      'v-intro':intro
    },
    methods:{
      closeIntro(){
          this.intro = false
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/style">
  @import "common/stylus/index.styl";
</style>
<style lang="less">
  @import '~vux/src/styles/reset.less';
</style>

