<template>
  <transition name="foodDetail">
    <div class="foodDetail" v-show="showFlag">
      <scroller lock-x ref="scroller" :height="otherHeightCal+'px'" :scrollbarY='true'>
        <div class="scroll-wrap">
          <div class="icon">
            <img :src="food.image" alt="">
            <i class="el-icon-close" @click="toggleShow"></i>
          </div>
          <div class="wrap">
            <div class="title">
              <h3>{{food.name}}</h3>
              <span class="lala">月售{{food.sellCount}}   好评率{{food.rating}}%</span>
              <div class="price">
                <span class="price">¥{{food.price}}</span>
                <span v-if="food.oldPrice" class="oldPrice">¥{{food.oldPrice}}</span>
              </div>
              <div class="addToCart">
                <el-button v-if="food.count==0 || food.count==undefined" @click="addToCart" type="info" size="small">
                  加入购物车
                </el-button>
                <cartCount v-if="food.count>0" :food="food" @add="addFood"></cartCount>
              </div>
            </div>
            <gapper></gapper>
            <div class="text" v-if="food.info">
              <p>商品介绍：</p>
              <p>{{food.info}}</p>
            </div>
            <gapper v-if="food.info"></gapper>
          </div>
          <div class="ratingsWrap">
            <h3 class="title">商品评价</h3>
            <ratingSelect @select="selectTypeEv"
                          @toggle="contentToggleEv"
                          :ratings="food.ratings"
                          :select-type="selectType"
                          :only-content="onlyContent"
                          :desc="desc"
            >
            </ratingSelect>
            <ratingwrap
              :select-type="selectType"
              :only-content="onlyContent"
              :ratings="food.ratings"></ratingwrap>
          </div>
        </div>
      </scroller>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import gapper from 'components/gapper/gapper'
  import cartCount from 'components/cartCount/cartCount';
  import {Scroller} from 'vux';
  import Vue from 'vue'
  import ratingSelect from 'components/ratingSelect/ratingSelect'
  import ratingwrap from 'components/ratingwrap/ratingwrap'
  const ALL = 2;
  export default{
    props: {
      food: {
        type: Object
      }
    },
    data(){
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: false,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      }
    },
    created(){

    },
    mounted(){

    },
    computed: {
      otherHeightCal(){
        return ( window.screen.height - 20)
      },
    },
    methods: {
      show(){
        this.showFlag = true;
        this.$nextTick(() => {
          setTimeout(() => {
            this.$refs.scroller.reset();
          }, 200)
        })
      },
      toggleShow(){
        this.showFlag = !this.showFlag;
      },
      addToCart(event){
        Vue.set(this.food, 'count', 1)
        this.$emit('add', event.target)
      },
      addFood(target) {
        this.$emit('add', target);
      },
      selectTypeEv(type){
        this.selectType = type;
        this.$nextTick(() => {
          this.$refs.scroller.reset();
        })
      },
      contentToggleEv(){
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          this.$refs.scroller.reset();
        })
      }
    },
    components: {
      gapper,
      Scroller,
      cartCount,
      ratingSelect,
      ratingwrap
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .foodDetail
    position fixed
    left 0
    top 0
    bottom .48rem
    z-index 30
    width 100%
    background #fff
    transition all 1s linear
    .icon
      width 100%
      /*position relative*/
      .el-icon-close
        position: fixed;
        top: .5rem;
        right: .5rem;
        font-size: .5rem;
        color: rgba(0, 0, 0, .4);
      img
        width 100%
    .title
      position relative
      padding 10px
      .lala
        display inline-block
        color #aaa
        padding .4rem 0
      .price
        font-size .4rem
        color red
      .oldPrice
        color #aaa
        text-decoration line-through
        font-size .2rem
      .addToCart
        position absolute
        right .5rem
        top .5rem

  .foodDetail-enter-active {
    transition all 1s ease
  }

  .foodDetail-leave-active {
    transition all 1s cubic-bezier(1.0, 0.5, 0.8, 1.0)
  }

  .foodDetail-enter, .foodDetail-leave {
    transform translateY(10px)
    opacity 0
  }
</style>
