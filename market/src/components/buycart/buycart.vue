<template>
  <div class="goods-footer" ref="footer">
    <div class="cart">
      <el-badge :value="totalCount"
                class="item cart-icon"
                :class="{'cartHt':totalCount>0}">
        <i class="iconfont vfont-gouwuche"></i>
      </el-badge>
      <span class="cart-total">¥{{totalPrice}}</span>
    </div>
    <div class="others">
      另需配送费¥{{deliveryPrice}}元
    </div>
    <div class="total-price" :class="{'payment':payClass==1}">
      {{payDesc}}
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
    props: {
      selectedFoods: {
        type: Array,
        default(){
          return [
            {
              price: 30,
              count: 2
            }
          ]
        }
      },
      deliveryPrice: {
        type: [Number],
        default: 0
      },
      minPrice: {
        type: [Number],
        default: 0
      }
    },
    methods: {},
    computed: {
      totalPrice(){
        let total = 0;
        this.selectedFoods.forEach((food) => {
          total += food.price * food.count;
        })
        return total;
      },
      totalCount(){
        let count = 0
        this.selectedFoods.forEach((food) => {
          count += food.count;
        })
        return count;
      },
      payDesc(){
        console.log(this.totalPrice, this.minPrice);
        if (this.totalPrice === 0) {
          return `¥ ${this.minPrice}元起送`
        } else if (this.tatolPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差¥${diff}元起送！`
        } else {
          return "去结算"
        }
      },
      payClass(){
        if(this.tatolPrice < this.minPrice) {
          return 0
        } else {
          return 1
        }
      }
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
      display: flex;
      .el-badge
        sup
          right .3rem
          top .1rem
      .cart-icon
        &.cartHt{
          background indianred
        }
        width: 1.2rem;
        height: 1.2rem;
        position relative
        display inline-block
        top: -0.4rem;
        left: 0.1rem;
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
      &.payment{
        background indianred
        color: #fff
      }
      height: 100%
      width 2.01rem
      font-size .24rem
      color rgba(255, 255, 255, 0.4)
      font-weight 700rem
      line-height .96rem
      background #2b333b
      text-align center

</style>
