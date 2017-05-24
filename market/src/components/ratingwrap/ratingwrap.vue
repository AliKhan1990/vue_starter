<template>
  <div class="wrap">
    <ul v-if="ratings">
      <li class="rating" v-for="rating in ratings">
        <div class="avatar">
          <img :src="rating.avatar" alt="">
        </div>
        <div class="ratingBox">
          <div class="userAndTime">
            <div class="name">{{rating.username}}</div>
            <time>{{formatDate(rating.rateTime)}}</time>
          </div>
          <div class="star">
            <el-rate
              v-model="rating.score"
              disabled
              text-color="#ff9900"
              text-template="{value}">
            </el-rate>
            <p v-if="rating.deliveryTime">用时{{rating.deliveryTime}}分钟送达</p>
          </div>
          <i :class="[rating.rateType===0 ? 'el-icon-check':'el-icon-close']"></i>
          <p class="text">
            {{rating.text}}
          </p>
          <div class="recommend">
            <span v-for="item in rating.recommend">{{item}}</span>
          </div>
        </div>
      </li>
    </ul>
    <div v-else="!ratings" class="no-rating">
      <p>暂无评论！亲，留下第一条评论吧～</p>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default{
    props: {
      ratings: {}
    },
    created(){

    },
    methods: {
      formatDate(time){
        let date = new Date(time);
        let Y = date.getFullYear() + '-';
        let M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
        let D = date.getDate() + ' ';
        let h = date.getHours() + ':';
        let m = date.getMinutes() + ':';
        let s = date.getSeconds();
        return Y + M + D + h + m + s;
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .rating
    padding-top 10px
    border-bottom 1px solid #eee
    display flex
    flex-direction row
    .avatar
      width 1.1rem
      img
        border-radius 50%
        width .8rem
        height .8rem
    .userAndTime
      time
        color #c3c6c9
      display flex
      flex-direction row
      justify-content space-between
    .ratingBox
      width 100%
    .star
      .el-rate
        .el-rate__item
          i
            font-size 10px
          p
            color #c3c6c9
      padding-top .25rem
      display flex
    .recommend
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      padding .16rem 0 .36rem 0
      span
        margin .2rem
        background: #eee;
        border: 1px #aaa solid;
        border-radius: 10rem;
        padding: 2px;
        color: #aaa;
        margin: 2px 3px;
</style>


