<template>
  <div class="goods-footer" ref="footer">
    <div class="cart">
      <div class="cart-icon">
        <i class="iconfont vfont-gouwuche"></i>
      </div>
      <span class="cart-total">¥0</span>
    </div>
    <div class="others">
      另需配送费¥4元
    </div>
    <div class="total-price">
      ¥20起送
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import eventBus from '../../assets/eventBus'
  export default{
    data(){
      return {
        cartHeight: 0
      }
    },
    methods: {
      buycartHeight(){
          let self = this;
        this.cartHeight = this.$refs.footer.clientHeight;
        eventBus.$emit("sendFromCart", self.cartHeight);
        //console.log('购物车'+this.cartHeight);
      }
    },
    mounted(){
      this.buycartHeight();
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .goods-footer
    display flex
    position fixed
    bottom 0
    left 0
    width 100%
    height 1rem
    background #141d27
    .cart
      .cart-icon
        width: 1.2rem;
        height: 1.2rem;
        position relative
        display inline-block
        top: -0.4rem;
        left: 0.5rem;
        border-radius: 50%;
        color: rgb(255, 255, 255, .4)
        font-size .48rem
        line-height .48rem
        background #141d27
        i
          font-size: .78rem;
          border-radius: 50%;
          position: relative;
          top: .45rem;
          left: .2rem;
      .cart-total
        display inline-block
        width: 1rem;
        text-align center
        font-size .32rem
        color rgba(255, 255, 255, .4)
        font-weight 700
        line-height 1rem
        position relative
        z-index 30
        border-right 1px solid rgba(255, 255, 255, 0.1)
    .others
      font-size .32rem
      color rgba(255, 255, 255, .4);
      font-weight 700
      line-height 1rem
      padding 0 .44rem
    .total-price
      height: 100%
      width 2rem
      font-size .24rem
      color rgba(255, 255, 255, 0.4)
      font-weight 700rem
      line-height .96rem
      background #2b333b
      text-align center

</style>
