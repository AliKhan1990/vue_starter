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
        dropBalls: []
      }
    },
    methods: {
      drop(el) {
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
      }
    },
    computed: {
      totalPrice(){
        let total = 0;
        this.selectedFoods.forEach((food) => {
          if (food.count != undefined) {
            total += food.price * food.count;
          }
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
    z-index 50
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
      width 2.01rem
      font-size .24rem
      color rgba(255, 255, 255, 0.4)
      font-weight 700rem
      line-height .96rem
      background #2b333b
      text-align center
    .ball-wrap
      .ball
        position: fixed
        left: 32px
        bottom: 22px
        z-index: 200
        transition: all 0.4s cubic-bezier(.33,1.01,.93,-0.04)
        .inner
          width: 16px
          height: 16px
          border-radius: 50%
          background: rgb(0, 160, 220)
          transition: all 0.4s linear


</style>
