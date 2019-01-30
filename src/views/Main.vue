<template>
  <div class="main">
    <el-container>
      <!--左侧的侧边栏-->
      <el-aside width="200px">
        <el-menu>
          <el-menu-item index="1">
            <i class="el-icon-document"></i>
            <span>全局设置</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-container>
        <!--顶部的信息栏-->
        <el-header height="60px">欢迎回来{{$store.state.adminName}}</el-header>
        <!--主体部分-->
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
 export default{
   //create/mount/update/distory  
   beforeCreate(){
     //组件创建之前，检查是否登录
     if(!this.$store.state.adminName){
       this.$router.push("/login");  //未登录则跳转
     }
   }
 }
</script>

