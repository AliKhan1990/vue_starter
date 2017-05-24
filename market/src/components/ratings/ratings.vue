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
                text-template="{value}">
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
    <scroller ref="filterRating" lock-x class="satisficing" @on-scroll="onScroll">
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
    <div class="arrowTo" v-show="arrowTop" @click="toRatingTop">
      <img src='./img/arrowTop.svg' alt="">
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import ratingwrap from 'components/ratingwrap/ratingwrap';
  import ratingSelect from 'components/ratingSelect/ratingSelect'
  import {Scroller} from 'vux';
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
      Scroller,
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
    },
    methods: {
      onScroll(pos){
        if (pos.top >= 10) {
          this.arrowTop = true;
        } else {
          this.arrowTop = false;
        }
      },
      _initScroller(){
        this.$refs.filterRating.reset({top:10});
      },
      selectTypeEv(type){
        this.selectType = type;
        this.$nextTick(() => {
            setTimeout(()=>{
              this.$refs.filterRating.reset();
            },700)
        })
      },
      contentToggleEv(){
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          setTimeout(()=>{
            this.$refs.filterRating.reset();
          },700)
        })
      },
      toRatingTop(){
        this.$refs.filterRating.reset({top: 0},300,'ease-in');
        this.arrowTop = false;
        setTimeout(()=>{
          this.$refs.filterRating.reset();
        },200)
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


</style>
