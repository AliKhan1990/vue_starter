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
    <scroller ref="filterRating" lock-x class="satisficing">
      <div class="sbVux">
        <div class="bar">
          <el-button type="primary" @click="allRatings" size="mini">全部  {{ratingCount('all')}}</el-button>
          <el-button type="success" @click="goodRatingsFilter" size="mini">满意  {{ratingCount('yes')}}</el-button>
          <el-button type="warning" @click="badRatingsFilter" size="mini">不满意 {{ratingCount('no')}}</el-button>
        </div>
        <div class="have-text">
          <el-button size="mini">
            <i class="el-icon-check"></i>
            只看有内容的评价
          </el-button>
        </div>
        <div class="ratingsWrap" v-for="item in ratings">
          <ratingwrap :rating="item"></ratingwrap>
        </div>
      </div>
    </scroller>
  </div>
</template>

<script type="text/ecmascript-6">
  import ratingwrap from 'components/ratingwrap/ratingwrap';
  import {Scroller} from 'vux';
  export default{
    data(){
      return {
        ratingsFilter: []
      }
    },
    components: {
      ratingwrap,
      Scroller
    },
    props: {
      seller: {
        type: Object
      },
      ratings: {
        type: Array
      }
    },
    mounted(){
      this.ratingsFilter = this.ratings;
      setTimeout(() => {
        this._initScroller()
      }, 200)
    },
    methods: {
      ratingCount(kind){
        let good = 0;
        let bad = 0;
        let all = good + bad;
        if (kind == 'yes') {
          this.ratings.forEach((rating) => {
            if (rating.score >= 3.5) {
              good++
            }
          })
          return good;
        } else if (kind == 'no') {
          this.ratings.forEach((rating) => {
            if (rating.score < 3.5) {
              bad++
            }
          })
          return bad;
        } else {
          this.ratings.forEach((rating) => {
            if (rating.score) {
              all++
            }
          })
          return all;
        }
      },
      allRatings(){
        if (this.ratingsFilter.length == 0) {
          this.ratingsFilter = this.ratings;
        }else{

        }
      },
      goodRatingsFilter(){
        if (this.ratingsFilter.length == 0) {
          this.ratingsFilter = this.ratings;
        }
      },
      badRatingsFilter(){

      },
      _initScroller(){
        this.$refs.filterRating.reset();
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

</style>
