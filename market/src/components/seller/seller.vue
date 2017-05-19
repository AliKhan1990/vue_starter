<template>
  <scroller class="grid-content" lock-x ref="sellerScroller" :scrollbarY='true' :height="otherHeightCal+'px'">
    <div class="seller">
      <div class="intro">
        <div class="intro-top">
          <div class="title">
            <h3>{{seller.name}}</h3>
            <div class="rate">
              <el-rate
                v-model="seller.foodScore"
                disabled
                allow-half
                show-text
                text-color="#ff9900"
                text-template="{value}">
              </el-rate>
              <span class="ratepeople">
              ({{seller.ratingCount}}评价)
            </span>
              <span class="sellCount">月售{{seller.sellCount}}件</span>
            </div>
          </div>
          <div class="collect">
            <i class="el-icon-star-off"></i>
          </div>
        </div>
        <div class="intro-bot">
          <div class="start-price">
            <span>起送价</span>
            <div class="wp">
              {{seller.minPrice}}
              <span class="yuan">元</span>
            </div>
          </div>
          <div class="carriage">
            <span>商家配送</span>
            <div class="wp">
              {{seller.deliveryPrice}}
              <span>元</span>
            </div>
          </div>
          <div class="average-time">
            <span>平均配送事件</span>
            <div class="wp">
              {{seller.deliveryTime}}
              <span>分钟</span>
            </div>
          </div>
        </div>
      </div>
      <div class="notice">
        <h3 class="title">
          公告与活动
        </h3>
        <p>{{seller.bulletin}}</p>
        <ul class="supports">
          <li v-for="(support,index) in seller.supports">
            <span class="icon" :class="classMap[support.type]"></span>
            <p>{{support.description}}</p>
          </li>
        </ul>
      </div>
      <div class="picture">
        <h3>商家实景</h3>
        <el-carousel indicator-position class="picWrap" type="card" height="4rem">
          <el-carousel-item v-for="item in seller.pics" :key="item">
            <img :src="item" alt="">
          </el-carousel-item>
        </el-carousel>
      </div>
      <div class="more-info">
        <h3>商家信息</h3>
        <ul class="info">
          <li v-for="item in seller.infos">{{item}}</li>
        </ul>
      </div>
    </div>
  </scroller>
</template>

<script type="text/ecmascript-6">
  import {Scroller, Swiper, SwiperItem} from 'vux';
  export default{
    props: {
      seller: {}
    },
    components: {
      Scroller,
      Swiper,
      SwiperItem
    },
    data(){
      return {}
    },
    created(){
      this.idx = 0;
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    mounted(){
      setTimeout(() => {
        this._initScroll()
      }, 200)
    },
    methods: {
      _initScroll(){
        this.$refs.sellerScroller.reset({top: 0})
      }
    },
    computed: {
      otherHeightCal(){
        return (window.screen.width / 7.5 * 9.5 - window.screen.height +10)
      },
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin"
  .seller
    height 100%
    & > div
      background white
      border-bottom 1px solid #ddd
      padding .3rem
      margin-bottom .3rem

    background #eee
    display flex
    flex-direction column
    .intro
      .intro-top
        justify-content: space-around;
        border-bottom: 1px solid #eee;
        padding-bottom: .2rem;
        display flex
        .title
          h3
            font-size .3rem
            line-height .6rem

        .collect
          font-size: .7rem;
          color: #aaa;

        .rate
          display flex
          .ratepeople
            padding 0 .2rem

          i
            font-size .12rem

      .intro-bot
        padding-top: .4rem;
        padding-bottom .4rem
        display flex
        text-align center
        & > div
          width 33.333%
          border-right 1px solid #eee
          &:last-of-type
            border: 0

          & > span
            color #aaa

        .wp
          font-size .4rem
          padding 10px
          text-align center
          span
            font-size .1rem

    .notice
      & > p
        padding-top .16rem
        padding-bottom .32rem
        color rgb(240, 20, 20)
        font-size .24rem
        font-weight 200
        line-height .48rem
      .supports
        li
          p
            padding-left .12rem
          display flex
          text-align center
          border-bottom solid 1px rgba(7, 17, 27, 0.1)
          font-size .24rem
          line-height .48rem
          font-weight 200
          color rgb(7, 17, 27)
          .icon
            display inline-block
            border-radius .04rem
            width .24rem
            height .24rem
            background-size .24rem .24rem
            &.decrease
              bg-img('decrease_3')
            &.discount
              bg-img('discount_3')
            &.guarantee
              bg-img('guarantee_3')
            &.invoice
              bg-img('invoice_3')
            &.special
              bg-img('special_3')
    .picture
      h3
        padding .36rem 0 .24rem 0
      .el-carousel__indicators
        height auto
      .picWrap
        margin-top .4rem
        img
          width 4rem
          height 3rem
          margin .1rem
    .more-info
      h3
        padding .36rem 0 .24rem 0
      ul
        li
          padding .32rem 0
          border-top 1px solid #eee
          border-bottom 1px solid #eee


</style>
