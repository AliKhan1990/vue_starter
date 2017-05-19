<template>
  <div id="view">
    <v-intro v-on:enter="closeIntro" v-show="!intro"></v-intro>
    <v-header v-show="intro" :seller="seller"></v-header>
    <v-tab v-show="intro" :seller="seller" :ratings="ratings"></v-tab>
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
        ratings: [],
        intro: true
      }
    },
    created(){
      this.getSeller();
      this.getRatings();
    },
    mounted(){

    },
    components: {
      "v-header": header,
      "v-tab": tab,
      'v-intro': intro
    },
    methods: {
      closeIntro(){
        this.intro = false
      },
      getSeller(){
        this.axios.get("/api/seller").then(res => {
          res = res.data;
          if (res.errno == ERR_OK) {
            this.seller = res.data;
          }
        });
      },
      getRatings(){
        this.axios.get("/api/ratings").then(res => {
          res = res.data;
          if (res.errno == ERR_OK) {
            this.ratings = res.data;
          }
        });
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

