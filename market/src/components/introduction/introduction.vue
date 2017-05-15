<template>
  <el-carousel trigger="click" :autoplay="false" arrow="never" height="16.30rem" :interval="4000">
    <el-carousel-item class="intro-item" v-for="item in intro" :key="item">
      <h3>{{ item.title }}</h3>
      <p>{{item.text}}</p>
      <el-button v-if="item.button" @click="enter" type="primary">{{item.button}}</el-button>
    </el-carousel-item>
  </el-carousel>
</template>

<script type="text/ecmascript-6">
export default{
    data(){
        return {
            intro:[
              {title:'Hello !',text:'Yooooo!'},
              {title:'Hello !',text:'Yooooo!'},
              {title:'Hello !',text:'Yooooo!'},
              {title:'Hello !',text:'Yooooo!',button:'Enter'},
            ]
        }
    },
  methods:{
        enter(){
          this.$emit('enter')
        }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
.intro-item{
  background #eee
}
.el-carousel__indicators{
  text-align: center;
  display: inline-block;
  height: 4rem;
}
</style>
