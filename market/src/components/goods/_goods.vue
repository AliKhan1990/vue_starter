<template>
  <el-row>
    <el-col :span="5" class="menu-wrap">
      <div class="grid-content">
        <!--需要给父级加相对定位-->
        <scroller lock-x  ref="menuScroller" height="-200" @on-scroll="flScroll">
          <ul class="kinds-list" v-scroll="fsScroll">
            <li class="list-item" v-if="goods" v-for="(item,index) in goods"
                :class="{'current':currentIdx===index}"
                @click="listClick(index)">
              <div class="goods">
                <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
              </div>
            </li>
          </ul>
        </scroller>
      </div>
    </el-col>
    <el-col :span="19" class="foods-wrap">
      <div class="grid-content">
        <scroller class="goods-wrap" lock-x @on-scroll="fsScroll" ref="foodsScroller">
          <ul class="goods-list">
            <li class="good" ref="goodHook" v-for="item in goods">
              <h1 class="title">{{item.name}}</h1>
              <ul class="foods-wrap">
                <li class="food" v-if="item.foods" v-for="food in item.foods">
                  <div class="icon">
                    <img :src="food.icon" alt="食品">
                  </div>
                  <div class="details">
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
                    <!--<span class="el-icon-plus"></span>-->
                  </div>
                </li>
              </ul>
            </li>
          </ul>
        </scroller>
      </div>
    </el-col>
    <buy-cart></buy-cart>
  </el-row>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import buyCart from 'components/buycart/buycart';
  //Vux
  import {
    Scroller
  } from 'vux'
  const ERR_OK = 0;
  export default{
    prop: ['seller', 'headerHeight'],
    components: {
      Scroller,
      buyCart
    },
    data(){
      return {
        goods: [],
        scrollTop: 0,
        listHeight: [0],
        minus: -200
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    mounted(){
      this.axios.get('/api/goods').then((res) => {
        res = res.data;
        if (res.errno == ERR_OK) {
          this.goods = res.data;
          this.$nextTick(() => {
            //等待渲染完毕开始重置及计算
            setTimeout(() => {
              this.$refs.foodsScroller.reset()
              this.$refs.menuScroller.reset()
              this._calculateHeight()
            }, 100);
          })
        }
      })
      this.$refs.goodScroller.addEventListener('scroll',()=>{
        console.log(111);})
    },
    methods: {
      _calculateHeight(){
        let goodList = this.$refs.goodHook;
        let height = 0;
        for (let i = 0; i < goodList.length; i++) {
          height += goodList[i].clientHeight;
          this.listHeight.push(height);
        }
        console.log(this.listHeight);
      },
      _acceptMinusHeight(){
        let headerHeight = 0;
        let tabsHeight = 0;
        let buyHeight = 0;
        console.log("accept")
        //from header component--heightEvent~|
        eventBus.$on('sendFromHeader', (hh) => {
          console.log(hh);
          headerHeight = hh;
        })
        //from tabs component--heightEvent~|
        eventBus.$on('sendFromTabs', (th) => {
          tabsHeight = th;
        })
        //from buycat component--heightEvent~|
        eventBus.$on('sendFromCart', (bh) => {
          buyHeight = bh;

        })
        this.minus = -(headerHeight + tabsHeight + buyHeight) + " ";
        //console.log(headerHeight,tabsHeight,buyHeight);
      },
      fsScroll(pos){
          console.log(1111)
//        this.scrollTop = pos.top+10;
      },
      flScroll(pos){

      },
      listClick(index){
        this.scrollTop = this.listHeight[index]+3;
        this.$refs.foodsScroller.reset({top: this.listHeight[index]},300)
      }
    },
    computed: {
      currentIdx(){
        for (let i = 0; i < this.listHeight.length; i++) {
          let heightF = this.listHeight[i];
          let heightN = this.listHeight[i + 1];
          console.log(this.scrollTop);
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

  .foods-wrap
    .goods-wrap
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
                .el-icon-plus
                  position absolute
                  right .3rem
                  font-size .3rem
                  line-height .5rem
                  width .5rem
                  text-align center
                  border-radius 50%
                  color white
                  background rgb(0, 160, 220)


</style>
