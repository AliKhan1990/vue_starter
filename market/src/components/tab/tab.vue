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
    <router-view keep-live :ratings="ratings" :seller="seller"></router-view>
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
      seller:{
          type:Object
      },
      ratings:{
          type:Array
      }
    },
    methods: {
      sendTabsHeight(){
        let self = this;
        this.tabsHeight = this.$refs.tab.clientHeight;
        eventBus.$emit("sendFromTabs", self.tabsHeight);
      }
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
    border-1px(rgba(7, 27, 37, 0.3));
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
