<template>
  <el-row>
    <el-col :span="5" class="menu-wrap">
      <scroller class="grid-content" ref="menuScroller" lock-x :height="otherHeightCal+'px'">
        <ul class="kinds-list">
          <li class="list-item" v-if="goods" v-for="(item,index) in goods"
              :class="{'current':currentIdx===index}"
              @click="listClick(index)">
            <div class="goods">
              <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
            </div>
          </li>
        </ul>
      </scroller>
    </el-col>
    <el-col :span="19" class="foods-wrapper">
      <scroller class="grid-content" ref="foodScroller" @on-scroll="fsScroll" lock-x :scrollbarY='true'>
        <ul class="goods-list">
          <li class="good" ref="goodHook" v-for="item in goods">
            <h1 class="title">{{item.name}}</h1>
            <ul class="foods-wrap">
              <li class="food" v-if="item.foods" v-for="food in item.foods">
                <div class="icon">
                  <img v-if="food.icon" :src="food.icon" alt="食品">
                </div>
                <div class="details">
                  <div @click="clickedFood(food)" :food="food" class="foodDetailWrap">
                    <h3 class="title">{{food.name}}</h3>
                    <div class="desc">{{food.description}}</div>
                    <span class="saled">月售{{food.sellCount}}份／</span>
                    <span class="goodpin">好评{{food.rating}}%</span>
                    <div class="price">
                      <span class="now">
                        <span class="rmb">¥</span>{{food.price}}
                      </span>
                      <span v-if="food.oldPrice" class="past">
                        <span class="rmb">¥</span>{{food.oldPrice}}
                      </span>
                    </div>
                  </div>
                  <div class="count-wrap">
                    <cartCount @add="addFood" :food="food"></cartCount>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </scroller>
    </el-col>
    <buy-cart ref="buyCart" :selected-foods='selectedFoods'
              :seller="seller"
              :delivery-price="seller.deliveryPrice"
              :min-price="seller.minPrice" :galaxy-foods="galaxyFoods">
    </buy-cart>
    <food :food="clickFood" @add="addFood" ref="food"></food>
  </el-row>
</template>
<script type="text/ecmascript-6">
  import {Scroller} from 'vux';
  import buyCart from 'components/buycart/buycart';
  import cartCount from 'components/cartCount/cartCount';
  import food from 'components/food/food'
  const ERR_OK = 0;
  export default{
    props: {
      seller: Object
    },
    components: {
      buyCart,
      Scroller,
      cartCount,
      food
    },
    data(){
      return {
        goods: [],
        scrollTop: 0,
        listHeight: [0],
        minus: -200,
        selectedFood: {},
        clickFood: {}
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.axios.get('/api/goods').then((res) => {
        res = res.data;
        if (res.errno == ERR_OK) {
          this.goods = res.data;
          this.$nextTick(() => {
            //等待渲染完毕开始重置及计算
            setTimeout(() => {
              this._calculateHeight()
              this._initScroll()
            }, 100)
          })
        }
      })
    },
    mounted(){
      this.$nextTick(() => {
        setTimeout(() => {
          this._initScroll()
        }, 100)
      })
    },
    methods: {
      fsScroll(pos){
        this.scrollTop = pos.top;
      },
      addFood(target) {
        this._drop(target);
      },
      _drop(target) {
        // 体验优化,异步执行下落动画
        this.$nextTick(() => {
          //ref自带它组件
          this.$refs.buyCart.drop(target);
        });
      },
      _calculateHeight(){
        let goodList = this.$refs.goodHook;
        let height = 0;
        for (let i = 0; i < goodList.length; i++) {
          height += goodList[i].clientHeight;
          this.listHeight.push(height);
        }
      },
      _initScroll(){
        this.$refs.menuScroller.reset({top: 0})
        this.$refs.foodScroller.reset({top: 0})
      },
      listClick(index){
        this.$refs.foodScroller.reset({top: this.listHeight[index]})
        this.scrollTop = this.listHeight[index];
      },
      addFood(target) {
        console.log(target);
        this._drop(target);
      },
      _drop(target) {
        // 体验优化,异步执行下落动画
        this.$nextTick(() => {
          this.$refs.buyCart.drop(target);
        });
      },
      clickedFood(food){
        this.clickFood = food;
        this.$refs.food.show();
        this.foodEnter = true;
      }
    },
    computed: {
      selectedFoods(){
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            foods.push(food)
          })
        })
        return foods;
      },
      galaxyFoods(){
        let noFilter = this.selectedFoods;
        let filter = [];
        noFilter.forEach((food) => {
          if (food.count) {
            filter.push(food);
          } else {
            return false;
          }
        })
        return filter;
      },
      otherHeightCal(){
        return (window.screen.width / 7.5 * 8.8)
      },
      currentIdx(){
        //因为有过渡动效,所以scrollTop计算会有延迟
        for (let i = 0; i < this.listHeight.length; i++) {
          let heightF = this.listHeight[i];
          let heightN = this.listHeight[i + 1];
          if (!heightN || (this.scrollTop >= heightF && this.scrollTop < heightN)) {
            return i;
          }
        }
        return 0;
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin";
  .menu-wrap
    position relative
    height 8.5rem
    .kinds-list
      list-style none
      margin 0
      padding 0
      .list-item
        border-bottom 0.001rem #ddd solid
        display: flex;
        justify-content: center;
        align-items: Center;
        &.current
          position relative
          margin-top -1px
          z-index 10
          background #fff
          font-weight 700
        &.active
          background #fff !important
        height 1.1rem
        font-size .24rem
        text-align center
        background #f3f5f7
        .icon
          vertical-align: middle;
          margin-left: 0.1rem;
          display inline-block
          border-radius .04rem
          width .27rem
          height .27rem
          background-size .27rem .27rem
          margin-right .05rem
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

  .foods-wrapper
    position relative
    height 8.5rem
    .goods-list
      position relative
      top 0
      list-style none
      padding 0
      margin 0
      .good
        > .title
          padding 0
          margin 0
          font-size .24rem
          color rgb(147, 153, 159)
          line-height .52rem
          background #f3f5f7
          border-left .04rem #d9dde1 solid
        .foods-wrap
          list-style none
          .food
            position relative
            display flex
            padding .36rem 0 0.36rem .36rem
            border-bottom .01rem solid #eee
            .icon
              width 1.15rem
              height 1.15rem
              img
                width 100%
            .details
              padding-left .2rem
              .title
                padding 0
                margin 0
                font-size .28rem
                color rgb(77, 85, 93)
                line-height .28rem
              .saled, .goodpin, .desc
                font-size .2rem
                color rgb(147, 153, 159)
                line-height .4rem
              .price
                .now
                  font-size .28rem
                  color red
                  font-weight 700
                  line-height .48rem
                  .rmb
                    font-size .15rem
                .past
                  text-decoration line-through
                  font-size .1rem
                  color #aaa
              .count-wrap
                position absolute
                bottom 10px
                right 10px
</style>
