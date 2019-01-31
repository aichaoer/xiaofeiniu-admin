<template>
  <div class="main">
    <el-container>
      <!--左侧的侧边栏-->
      <el-aside width="200px">
        <el-menu :default-active="defaultActive" :default-openeds="dafaultOpeneds" :unique-opened="true" :router="true">
          <!--1-->
          <el-menu-item index="/settings">
            <i class="el-icon-setting"></i>
            <span slot="title">全局设置</span>
          </el-menu-item>
          <!--2-->
          <el-submenu index="/table">
            <template slot="title">
              <i class="el-icon-menu"></i>
              <span>桌台管理</span>
            </template>
            <el-menu-item-group>
             <el-menu-item index="/table/list">桌台列表</el-menu-item>
             <el-menu-item index="/table/add">添加桌台</el-menu-item>
             <el-menu-item index="/table/delete">删除桌台</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <!--3-->
          <el-menu-item index="/category/list">
            <i class="el-icon-message"></i>
            <span>菜品类别</span>
          </el-menu-item>
          <!--4-->
          <el-submenu index="/dish">
            <template slot="title">
              <i class="el-icon-tickets"></i>
              <span>菜品管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="/dish/list">菜品列表</el-menu-item>
              <el-menu-item index="/dish/add">添加菜品</el-menu-item>
              <el-menu-item index="/dish/delete">删除菜品</el-menu-item>
              <el-menu-item index="/dish/update">修改菜品</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <!--5-->
          <el-menu-item index="/order/list">
            <i class="el-icon-date"></i>
            <span>订单列表</span>
          </el-menu-item>
          <!--6-->
          <el-menu-item index="/security">
            <i class="el-icon-warning"></i>
            <span>安全管理</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-container>
        <!--顶部的信息栏-->
        <el-header height="60px">
          <!--<MainHeader></MainHeader>-->
          <main-header></main-header>
        </el-header>
        <!--主体部分-->
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
import MainHeader from "../components/MainHeader"
 export default{
   //create/mount/update/distory  
   beforeCreate(){
     //组件创建之前，检查是否登录
     if(!this.$store.state.adminName){
       this.$router.push("/login");  //未登录则跳转
     }
   },
   data(){   //数据属性
     return { }
   },
   computed:{  //计算属性=数据属性+操作方法
     dafaultOpeneds(){
       if(this.$route.path.indexOf("/table")==0){
         //用户当前在浏览table/xxx菜单项
         return ["table"]
       }else if(this.$route.path.indexOf("/dish")==0){
         return ["dish"]
       }else{
         return [];
       }
     },
     defaultActive(){
       //用户当前访问哪一页，就把该页作为当前激活项
       return this.$route.path;
     }
   },
   components:{
     //当前组件内部使用的子组件列表
     MainHeader
   }
 }
</script>

<style lang="scss">
  .main{
     padding:50px 30px;
  }
</style>
