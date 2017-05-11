<template>
  <div class="goods-footer">
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
  export default{}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .goods-footer
    position fixed
    bottom 0
    left 0
    width 100%
    height .8rem
    background #141d27
    .cart
      .iconfont
        color:rgb(255,255,255,.4)
        font-size .48rem
        line-height .48rem
        background rgb(0,0,0,.2)
</style>
