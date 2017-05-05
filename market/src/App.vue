<template>
  <div id="view">
    <v-header :seller="seller"></v-header>
    <v-tab></v-tab>
  </div>
</template>

<script>
  import header from './components/header/header.vue';
  import tab from './components/tab/tab.vue';
  const ERR_OK = 0;
  export default{
      data(){
          return {
              seller:{}
          }
      },
      created(){
          this.$http.get("/api/seller").then(response=>{
              response = response.body;
              if(response.errno == ERR_OK){
                this.seller = response.data;
              }
          });
      },
      components:{
        "v-header":header,
        "v-tab":tab
      }
  }
</script>

<style  lang="stylus" rel="stylesheet/style">
  @import "common/stylus/index.styl";
</style>
