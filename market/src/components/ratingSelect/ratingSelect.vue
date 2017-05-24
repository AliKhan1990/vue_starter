<template>
  <div v-show="ratings.length>0" class="ratingSelect">
    <div class="raing-type">
      <el-button @click="selType(2)" type="primary" size="small">{{desc.all + "   " + ratings.length}}</el-button>
      <el-button @click="selType(0)" type="success" size="small">{{desc.positive+ "   " + positive.length}}</el-button>
      <el-button @click="selType(1)" type="warning" size="small">{{desc.negative+ "   " + negative.length}}</el-button>
    </div>
    <div class="switch">
      <el-button @click="toggleContent" size="mini">
        <i class="el-icon-circle-check"></i>
        只看有内容的评价
      </el-button>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const arr = [];
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const WALL = 3.5;
  const ALL = 2;
  export default{
    data(){
      return {}
    },
    props: {
      ratings: {
        type: Array,
        default(){
          return arr
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: true
      },
      desc: {
        type: Object,
        default(){
          return {
            all: '全部',
            positive: '满意',
            negative: '吐槽'
          }
        }
      }
    },
    computed: {
      positive(){
        return this.ratings.filter((rating) => {
          if(rating.rateType==POSITIVE){
              return true;
          }
        })
      },
      negative(){
        return this.ratings.filter((rating) => {
          if(rating.rateType==NEGATIVE){
            return true;
          }
        })
      }
    },
    methods: {
      selType(type){
        this.$emit('select',type)
      },
      toggleContent(){
        this.$emit('toggle')
      },
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .ratingSelect
    padding 10px
    .rating-type
      padding-bottom .2rem
    .switch
      padding .2rem 0
      border-bottom 1px solid #eee
</style>
