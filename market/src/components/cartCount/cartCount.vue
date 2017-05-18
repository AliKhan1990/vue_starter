<template>
  <div class="food_count">
    <transition name="minus">
      <span class="el-icon-minus" v-show="this.food.count" @click="minusFood"></span>
    </transition>
    <transition name="count">
      <el-input v-show="this.food.count!=undefined && this.food.count!=0" :value="food.count"></el-input>
    </transition>
    <span class="el-icon-plus" @click.stop.prevent="addFood"></span>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from "vue"
  export default{
    props: ['food'],
    methods: {
      minusFood(){
        if (this.food.count) {
          this.food.count--;
        }
      },
      addFood(){
        //添加食品
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
          this.food.count = 1
        } else {
          this.food.count++;
        }
        this.$emit('add', event.target)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .minus-enter-active, .count-enter-active {
    transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }

  .minus-leave-active, .count-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }

  .minus-enter, .minus-leave-active, .count-enter, .count-leave-active {
    transform: translate3D(10px, 0, 0);
    opacity: 0;
  }

  .food_count
    .el-input
      width 1rem
      input
        text-align center
        padding: 3px 10px;
        height: 25px;
    span[class*='el-icon']
      right .3rem
      font-size .3rem
      line-height .5rem
      width .5rem
      text-align center
      border-radius 50%
      color white
      background rgb(0, 160, 220)
</style>
