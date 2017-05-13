<template>
  <div id="view">
    <v-header :seller="seller"></v-header>
    <v-tab></v-tab>
  </div>
</template>

<script>
  import header from './components/header/header';
  import tab from './components/tab/tab';
  const ERR_OK = 0;
  export default{
    data(){
      return {
        seller: {},
        headerHeight:0
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
      "v-tab": tab
    },
    methods:{

    }
  }
</script>

<style lang="stylus" rel="stylesheet/style">
  @import "common/stylus/index.styl";
</style>

