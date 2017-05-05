<template>
  <el-row>
    <el-col :span="7" class="menu-wrap">
      <div class="grid-content">
        <ul class="kinds-list">
          <li class="list-item" v-if="goods" v-for="item in goods">
            {{item.name}}
          </li>
        </ul>
      </div>
    </el-col>
    <el-col :span="17" class="foods-wrap">
      <div class="grid-content">200000</div>
    </el-col>
  </el-row>
</template>

<script type="text/ecmascript-6">
  const ERR_OK = 0;
  export default{
    prop: ['seller'],
    data(){
        return{
            goods:[]
        }
    },
    created(){
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno == ERR_OK) {
            this.goods = response.data;
            console.log(this.goods)
        }
      })
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .grid-content
    .kinds-list
      list-style none
      margin 0
      padding 0
      .list-item
        height 1.1rem
        line-height 1.1rem
</style>
