<template>
  <div class="tab-view">
    <div class="tab border-1px" ref="tab">
      <div class="tab-item tab-active">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view keep-live :seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import eventBus from '../../assets/eventBus';
  export default{
    data(){
      return {
        tabsHeight: 0
      }
    },
    props:{
      seller:[Object]
    },
    methods: {
      sendTabsHeight(){
        let self = this;
        this.tabsHeight = this.$refs.tab.clientHeight;
        eventBus.$emit("sendFromTabs", self.tabsHeight);
        //console.log("tab" + this.tabsHeight);
      }
    },
    mounted(){

      this.sendTabsHeight();
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .tab
    display flex
    width 100%
    height .85rem
    line-height .85rem
    font-size .25rem
    border-1px(rgba(7, 17, 27, 0.1));
    .tab-item
      flex 1
      text-align center
      & > a
        display block
        text-decoration none
        &:hover, &.tab-active {
          color: deepskyblue
        }

</style>
