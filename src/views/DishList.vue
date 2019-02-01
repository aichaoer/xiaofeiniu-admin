<template>
  <div class="xfn-dish-list">
    <el-breadcrumb>
      <el-breadcrumb-item to="/main">首页</el-breadcrumb-item>
      <el-breadcrumb-item>菜品管理</el-breadcrumb-item>
      <el-breadcrumb-item>菜品列表</el-breadcrumb-item>
    </el-breadcrumb>
    <br>
    <el-tabs type="border-card">
      <el-tab-pane v-for="(c,i) in dishList">
      <span slot="label">
        <el-badge class="item" :value="c.dishList.length">{{c.cname}}</el-badge>
      </span>
      <el-row>
        <el-col :xs="12" :md="6" :lg="4" :xl="3" v-for="(d,j) in c.dishList">
          <!--<xfn-dish :data="d"></xfn-dish>-->
          {{d.title}}
          <img :src="require('../assets/img/dish/'+d.imgUrl)" alt="" style="max-width:90%" class="item">
        </el-col>
      </el-row>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
  export default{
    data(){
      return{
        dishList:[]   //[{cid:xx,cname:"",dishList:[……]}]
      }
    },
    mounted() {
      //异步获取菜品列表
      var url=this.$store.state.globalSettings.apiUrl+"/admin/dish";
      this.$axios.get(url).then(({data})=>{
        this.dishList=data;
      }).catch((err)=>{
        console.log(err);
      })
    },
  }
</script>
<style lang="scss">
  .item {
    margin-top: 10px;
    margin-right: 40px;
  }
</style>


