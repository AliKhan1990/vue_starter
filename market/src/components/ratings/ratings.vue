<template>
  <div class="all">
    <div class="ratings">
      <div class="total-rating">
        <div class="c-eval">
          <h4 class="score">{{seller.score}}</h4>
          <p>综合评分</p>
          <span>高于周边商家{{seller.rankRate}}</span>
        </div>
        <div class="service">
          <ul>
            <li>
              <span>食品评分</span>
              <el-rate
                v-model="seller.foodScore"
                disabled
                show-text
                text-color="#ff9900"
                text-template="{value}"
              >
              </el-rate>
            </li>
            <li>
              <span>服务态度</span>
              <el-rate
                v-model="seller.serviceScore"
                disabled
                show-text
                text-color="#ff9900"
                text-template="{value}">
              </el-rate>
            </li>
            <li>
              <span>平均送达时间</span>
              <p>{{seller.deliveryTime}}分钟</p>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <el-row class="ratingScroller">
      <scroller ref="filterRating" class="satisficing">
        <div class="sbVux">
          <ratingSelect @select="selectTypeEv"
                        @toggle="contentToggleEv"
                        :ratings="ratings"
                        :select-type="selectType"
                        :only-content="onlyContent"
                        :desc="desc"
          ></ratingSelect>
          <ratingwrap
            :ratings="ratings"
            :select-type="selectType"
            :only-content="onlyContent"
          >
          </ratingwrap>
        </div>
      </scroller>
    </el-row>

    <div class="arrowTo" v-show="arrowTop" @click="toRatingTop">
      <img src='./img/arrowTop.svg' alt="">
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import ratingwrap from 'components/ratingwrap/ratingwrap';
  import ratingSelect from 'components/ratingSelect/ratingSelect'
  const ALL = 2;
  export default{
    data(){
      return {
        selectType: ALL,
        onlyContent: false,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        },
        arrowTop: false
      }
    },
    components: {
      ratingwrap,
      ratingSelect
    },
    props: {
      seller: {
        type: Object
      },
      ratings: {
        type: Array
      }
    },
    created(){
      this.$nextTick(() => {
        setTimeout(() => {
          this._initScroller();
        }, 100)
      })
      this.timer = setInterval(() => {
        this.scroll_top = this.$refs.filterRating.getPosition().top
        if (this.scroll_top >= 5) {
          this.arrowTop = true;
        } else {
          this.arrowTop = false;
        }
      }, 100)
    },
    beforeDestroy(){
      clearInterval(this.timer);
    },
    methods: {
      _initScroller(){
        this.$refs.filterRating.resize();
      },
      selectTypeEv(type){
        this.selectType = type;
        this.$nextTick(() => {
          setTimeout(() => {
            this.$refs.filterRating.resize();
          }, 700)
        })
      },
      contentToggleEv(){
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          setTimeout(() => {
            this.$refs.filterRating.resize();
          }, 700)
        })
      },
      toRatingTop(){
        this.$refs.filterRating.scrollTo(0, 0, false);
        this.arrowTop = false;
        setTimeout(() => {
          this.$refs.filterRating.resize();
        }, 200)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .all
    background #f3f5f7
    & > div
      margin-bottom 20px
      background white

    .ratings
      .total-rating
        background white
        display flex
        flex-direction row
        .c-eval
          width: 35%;
          text-align center
          border-right 1px solid #eee
          .score
            padding .4rem 0 .2rem 0
            font-size .8rem
            font-weight 800
            color gold
          p
            padding-bottom .2rem
          span
            font-size .12rem
            color #aaa
        .service
          padding-top .5rem
          padding-left .4rem
          ul li
            & > span
              padding-right 5px
            p
              color #aaa
            height .7rem
            display flex
            .el-rate
              .el-rate__item i
                font-size 10px

    .satisficing
      .bar
        padding .36rem 0 .36rem 0
      .have-text
        padding .2rem
      border-bottom 1px solid #eee
    .sbVux
      padding .2rem

  .arrowTo
    position: fixed;
    z-index: 100;
    bottom: 1rem;
    right: .5rem;
    width: 1rem;
    img
      width 100%

  .ratingScroller
    height 7rem


</style>
