<template>
  <div class="header" ref="header">
    <div class="content-wrap">
      <div class="avatar">
        <img :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <h4 class="name">{{seller.name}}</h4>
        </div>
        <div class="desc">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[idx].type]"></span>
          <span class="text">{{seller.supports[idx].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" @click="showDetail" class="support-content">
        <span class="count">
          {{seller.supports.length}}
        </span>
        <span class="el-icon-arrow-right"></span>
      </div>
    </div>
    <div class="bulletin-wrap" @click="showDetail">
        <span class="bulletin-title">
          <span class="bulletin"></span>
        </span>
      <span v-if="seller.bulletin" class="bulletin-text">{{seller.bulletin.substr(0, 20) + "..."}}</span>
      <span class="el-icon-arrow-right"></span>
    </div>
    <div class="bk">
      <img :src="seller.avatar">
    </div>
    <transition name="detail">
      <div class="detail" v-if="seller" v-show="detailShow">
        <div class="detail-wrap">
          <div class="dt-main">
            <h4 class="dt-title">{{seller.name}}</h4>
            <div class="rateBox">
              <el-rate
                v-model="seller.foodScore"
                allow-half
                disabled
                show-text
                text-color="#ff9900"
                text="['极差', '失望', '一般', '满意', '惊喜']"
                text-template="{value}">
              </el-rate>
            </div>
            <div class="privilege">
              <h5 class="privilege-title">优惠信息</h5>
              <ul class="privilege-list">
                <li v-for="item in seller.supports">
                  <span class="icon" :class="classMap[item.type]"></span>
                  <span class="text">{{item.description}}</span>
                </li>
              </ul>
            </div>
            <div class="notice">
              <h5 class="notice-title">商家公告</h5>
              <span class="text">{{seller.bulletin}}</span>
            </div>
          </div>
        </div>
        <span class="el-icon-close" @click="showDetail"></span>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import bus from '../../assets/eventBus';
  export default{
    props: ['seller'],
    data(){
      return {
        detailShow: false,
        radio: 1,
        headerHeight: 10
      }
    },
    methods: {
      showDetail(){
        this.detailShow = !this.detailShow;
      }
    },
    created(){
      this.idx = 0;
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
  }
</script>
<style lang="stylus" rel="stylesheet/style">
  @import "../../common/stylus/mixin";
  body {
    font-size: .16rem
  }

  .header
    position: relative
    overflow: hidden
    color #fff
    background #999
    .content-wrap
      background: rgba(0, 0, 0, 0.5);
      position: relative;
      z-index: 20;
      padding .48rem .24rem .36rem .48rem
      position relative
      font-size 0
      .avatar
        display inline-block
        width 1.33rem
        height 1.33rem
        img
          width 100%
          height 100%
          border-radius: .1rem;
      .content
        display: inline-block
        margin-left .32rem
        font-size .32rem
        .title
          margin .04rem 0 .16rem 0
          .name
            display inline
            vertical-align: top;
            line-height .36rem
          .brand
            display inline-block
            width .6rem
            height .36rem
            bg-img('brand')
            background-repeat no-repeat
            background-size .6rem .36rem
            border-radius .04rem

        .desc
          font-size .24rem
          padding 0 0 .2rem 0

        .support
          font-size .2rem
          .text
            vertical-align top

          .icon
            display inline-block
            border-radius .04rem
            width .24rem
            height .24rem
            background-size .24rem .24rem
            &.decrease
              bg-img('decrease_2')
            &.discount
              bg-img('discount_1')
            &.guarantee
              bg-img('guarantee_1')
            &.invoice
              bg-img('invoice_1')
            &.special
              bg-img('special_1')

      .support-content
        position absolute
        right: .24rem
        bottom: .28rem
        padding: 0 .16rem
        height: .48rem
        line-height: .48rem
        border-radius: .28rem
        background: rgba(0, 0, 0, 0.2)
        text-align: center
        .count
          font-size .2rem
        .el-icon-arrow-right
          font-size .2rem

    .bulletin-wrap
      position: relative;
      z-index: 20;
      height: 0.56rem;
      line-height: 0.56rem;
      padding: 0 .44rem 0 .24rem
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      background: rgba(7, 17, 27, 0.7)

      .bulletin-title
        bg-img("bulletin")
        float: left
        margin-top 0.17rem
        background-size: .44rem .24rem
        background-repeat: no-repeat
        display: inline-block
        width: .44rem
        height: .24rem
      .bulletin-text
        font-size: .2rem
        width 6.25rem
        float: left
        margin-left 0.08rem
      .el-icon-arrow-right
        position: absolute;
        right: 0;
        font-size .16rem
        margin-top: .17rem;
        margin-right: .4rem;
    .bk
      position absolute
      top 0
      z-index 10
      width 100%
      img
        width 100%
        -webkit-filter: blur(.2rem)

    .detail
      position fixed
      z-index 100
      width 100%
      height 100%
      top 0
      left 0
      overflow auto
      background rgba(7, 17, 27, 0.8)
      .detail-wrap
        text-align center
        .dt-main
          .dt-title
            font-size .3rem
            text-align center
            margin-top 1.28rem
          .rateBox {
            margin-top .4rem
            margin-bottom: 0.5rem;
          }
          .privilege .privilege-title, .notice .notice-title
            margin-bottom .45rem
            font-size .28rem
            margin-top 0
            position relative
            &:before
              content: ''; /*CSS伪类用法*/
              position: absolute; /*定位背景横线的位置*/
              top: 45%;
              background: rgba(255, 255, 255, .7); /*宽和高做出来的背景横线*/
              width: 2rem;
              height: 1px;
              left 15%
            &:after
              content: ''; /*CSS伪类用法*/
              position: absolute; /*定位背景横线的位置*/
              top: 45%;
              background: rgba(255, 255, 255, .7); /*宽和高做出来的背景横线*/
              width: 2rem;
              height: 1px;
              right 15%
          .privilege .privilege-list, .notice .text
            list-style none
            text-align left
            padding-left .225rem
            padding-right .225rem
          .privilege
            .privilege-list
              margin: 0 auto;
              margin-bottom: 0.7rem;
              margin-left: 1rem;
              width 5.5rem
              li
                line-height .5rem
              .support
                font-size .2rem
              .text
                vertical-align top
              .icon
                display inline-block
                border-radius .04rem
                width .24rem
                height .24rem
                background-size .24rem .24rem
                &.decrease
                  bg-img('decrease_2')
                &.discount
                  bg-img('discount_1')
                &.guarantee
                  bg-img('guarantee_1')
                &.invoice
                  bg-img('invoice_1')
                &.special
                  bg-img('special_1')
          .notice .text
            width 5.5rem
            display inline-block
            line-height .5rem
      .el-icon-close
        position relative
        font-size .4rem
        width .4rem
        height .4rem
        margin .64rem auto 0 auto
        display block
    .detail-enter-active{
      transition all .3 ease;
    }
    .detail-enter-active{
        transition:all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
      }
    .detail-enter, .detail-leave-active {
      transition all .8s
      transform: translateX(10px);
      opacity: 0;
    }
</style>
