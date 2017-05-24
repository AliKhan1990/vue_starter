<template>
  <div class="cart-wrapper">
    <div class="goods-footer" ref="footer">
      <div class="front" @click="toggleList">
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
      </div>
      <el-button type="text" @click="payConfirm" class="total-price" :class="{'payment':payClass==1}">{{payDesc}}
      </el-button>
      <div class="ball-wrap">
        <div v-for="ball in balls">
          <transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
            <div class="ball" v-show="ball.show">
              <div class="inner inner-hook"></div>
            </div>
          </transition>
        </div>
      </div>
    </div>
    <transition name="cartList">
      <div class="shopcart-list" v-show="listFold">
        <el-table
          :data="galaxyFoods"
          show-overflow-tooltip
          stripe
          border
          style="width: 100%"
          empty-text="购物车空空如也～😢"
          maxHeight="200"
          @header-click='cleanCart'
        >
          <el-table-column
            label="商品列表"
            :width="screenWidth*.65"
            column-key="list"
          >
            <!--item-->
            <template scope="scope">
              <div class="food" v-if="scope.row.count
              && scope.row.count!=0
              && scope.row.count!==undefined ">
                <span class="name">{{scope.row.name.substr(0, 8)}}</span>
                <div class="price">¥{{scope.row.price}}</div>
              </div>
            </template>
          </el-table-column>
          <!--ctrl-->
          <el-table-column
            label="清空"
            :width="screenWidth*.35"
            column-key="clean">
            <template scope="scope">
              <cartCount v-if="scope.row.count
                               && scope.row.count!=0
                               && scope.row.count!==undefined"
                         :food="scope.row" @add="addFood">
              </cartCount>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </transition>
    <div class="list-mask" @click="toggleList" v-if="listFold"></div>
    <el-dialog
      :modal-append-to-body="true"
      top="15%"
      title="支付"
      :visible.sync="dialogVisible"
      size="large"
    >
      <span>共支付{{totalPrice + deliveryPrice + 1}}元。</span>
      <div>
        <h5>包含：</h5>
        <ul>
          <li>运费{{deliveryPrice}}元。</li>
          <li>包装费2元。</li>
        </ul>
      </div>
      <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="dialogVisible = false">支 付</el-button>
        </span>
    </el-dialog>

  </div>
</template>

<script type="text/ecmascript-6">
  import cartCount from 'components/cartCount/cartCount';
  export default{
    components: {
      cartCount
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
      },
      galaxyFoods: {
        type: Array
      }
    },
    data(){
      return {
        balls: [
          {show: false},
          {show: false},
          {show: false},
          {show: false},
          {show: false}
        ],
        dropBalls: [],
        listFold: false,
        cartHaveThing: false,
        dialogVisible: false,
      }
    },
    methods: {
      drop(el) {
        console.log(el);
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i];
          if (!ball.show) {
            ball.show = true;
            ball.el = el;
            this.dropBalls.push(ball);
            return;
          }
        }
      },
      addFood(target) {
        this.drop(target);
      },
      beforeDrop(el) {
        let count = this.balls.length;
        while (count--) {
          let ball = this.balls[count];
          if (ball.show) {
            let rect = ball.el.getBoundingClientRect();
            let x = rect.left - 32;
            let y = -(window.innerHeight - rect.top - 22);
            el.style.display = '';
            el.style.webkitTransform = `translate3d(0,${y}px,0)`;
            el.style.transform = `translate3d(0,${y}px,0)`;
            let inner = el.getElementsByClassName('inner-hook')[0];
            inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
            inner.style.transform = `translate3d(${x}px,0,0)`;
          }
        }
      },
      dropping(el, done) {
        /* eslint-disable no-unused-vars */
        let rf = el.offsetHeight;//强制刷新
        this.$nextTick(() => {
          el.style.webkitTransform = 'translate3d(0,0,0)';
          el.style.transform = 'translate3d(0,0,0)';
          let inner = el.getElementsByClassName('inner-hook')[0];
          inner.style.webkitTransform = 'translate3d(0,0,0)';
          inner.style.transform = 'translate3d(0,0,0)';
          el.addEventListener('transitionend', done);
        });
      },
      afterDrop(el) {
        let ball = this.dropBalls.shift();
        if (ball) {
          ball.show = false;
          el.style.display = 'none';
        }
      },
      toggleList(){
        if (this.totalCount != undefined) {
          this.listFold = !this.listFold;
        }
      },
      cleanCart(column){
        //handel current galaxy Data,Not its parent data-selectedFoods
        if (column.columnKey == 'clean' && this.totalCount > 0) {
          this.galaxyFoods.forEach((food) => {
            food.count = 0;
          })
        }
      },
      payConfirm(){
        if (this.payClass == 1 && this.totalPrice >= this.minPrice) {
          this.dialogVisible = true;
        }
      }
    },
    computed: {
      screenWidth(){
          return window.screen.width;
      },
      totalPrice(){
        let total = 0;
        this.galaxyFoods.forEach((food) => {
          total += food.price * food.count;
        })
        return total;
      },
      totalCount(){
        let counts = 0;
        this.selectedFoods.forEach((food) => {
          if (food.count != undefined) {
            counts += food.count;
          }
        });
        return counts;
      },
      payDesc(){
        if (this.totalPrice === 0) {
          return `¥ ${this.minPrice}元起送`
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差¥${diff}元起送！`
        } else if (this.totalPrice >= this.minPrice) {
          return "去结算"
        }
      },
      payClass(){
        if (this.totalPrice < this.minPrice) {
          return 0
        } else {
          return 1
        }
      },
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  cart-wrapper
    position relative

  .goods-footer
    display flex
    position fixed
    bottom 0
    left 0
    width 100%
    height 1rem
    background #141d27
    z-index 50
    .front
      display flex
    .cart
      display: flex;
      .el-badge
        sup
          right .3rem
          top .1rem
      .cart-icon
        &.cartHt {
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
      &.payment {
        background indianred
        color: #fff
      }
      height: 100%
      width 2.1rem
      font-size .24rem
      color rgba(255, 255, 255, 0.4)
      font-weight 700rem
      line-height .96rem
      background #2b333b
      text-align center
      span
        display: block;
        line-height: 0rem;
    .ball-wrap
      .ball
        position: fixed
        left: 32px
        bottom: 22px
        z-index: 200
        transition: all 0.4s cubic-bezier(.33, 1.01, .93, -0.04)
        .inner
          width: 16px
          height: 16px
          border-radius: 50%
          background: rgb(0, 160, 220)
          transition: all 0.4s linear

  .list-mask
    width: 100%;
    height: 100%;
    position: fixed;
    left: 0;
    top: 0;
    z-index: 30;
    background: rgba(0, 0, 0, 0.3);
    -webkit-filter blur(10px)
    filter blur(10px)

  .shopcart-list
    width 100%
    position fixed
    bottom 1rem
    z-index 40
    .el-table__header-wrapper thead tr th:nth-of-type(1) div {
      text-align center
    }
    .el-table__header-wrapper thead tr th:nth-of-type(2) div {
      color indianred
      text-align center
    }
    .el-table__body-wrapper .el-table_1_column_2 .cell {
      padding-right 0
    }
    .food
      display flex
      justify-content space-between
      .el-table .cell {
        white-space: normal;
        line-height: 24px;
      }

  /*购物和列表动画*/
  .cartList-enter-active {
    transition all .3 ease;
  }

  .cartList-enter-active {
    transition: all .3s cubic-bezier(.8, .11, .83, .67);
  }

  .cartList-enter, .cartList-leave-active {
    transition all .3s
    transform: translateY(10px);
    opacity: 0;
  }

  .el-message-box {
    width 5rem
  }

</style>
