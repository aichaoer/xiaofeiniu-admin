<template>
  <div class="xfn-category-list">
    <el-breadcrumb>
      <el-breadcrumb-item to="/main">首页</el-breadcrumb-item>
      <el-breadcrumb-item>菜单类别管理</el-breadcrumb-item>
      <el-breadcrumb-item>类别列表</el-breadcrumb-item>
    </el-breadcrumb>
    <br>
    <el-button type="primary" plain @click="addCotegory">+添加新的菜品类别</el-button>
    <el-table :data="categoryList" stripe border>
      <el-table-column label="编号" prop="cid"></el-table-column>
      <el-table-column label="名称" prop="cname"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="{row,$index}">
          <el-button type="success" size="small" @click="updateCotegory(row,$index)">修改</el-button>
          <el-button type="danger" size="small" @click="deleteCotegory(row,$index)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
  export default{
    data(){
      return {
        categoryList:[]
      }
    },
    mounted() {
      var url=this.$store.state.globalSettings.apiUrl+"/admin/category";
      this.$axios.get(url).then((res)=>{
        this.categoryList=res.data;
      }).catch((err)=>{
        console.log(err);
      })
    }, 
    methods:{
      addCotegory(){
        this.$prompt("请输入新得菜品名：","提示",{type:"info"}).then(({value})=>{
          //获得用户的输入,调用数据API添加
          var url=this.$store.state.globalSettings.apiUrl+"/admin/category";
          this.$axios.post(url,{cname:value}).then((res)=>{
            if(res.data.code==200){
              //数据库添加成功
              this.$message.success("新类别添加成功");
              //模型数据添加
              this.categoryList.push({cid:res.data.cid,cname:value})
            }else{
              this.$message.error("新类别添加失败："+res.data.msg);
            }
          }).catch((err)=>{
            console.log(err);
          })
        }).catch(()=>{})
      },
      deleteCotegory(c,i){
        this.$confirm("删除语句不可撤销,您确定吗？","提示",{type:"warning"}).then(()=>{
          var url=this.$store.state.globalSettings.apiUrl+"/admin/category/"+c.cid;
          this.$axios.delete(url).then((res)=>{
            if(res.data.code==200){
              //数据库中已经删除成功
              this.categoryList.splice(i,1);  //从模型数据中删除
              this.$message.success("菜品类别删除成功!");
            }else{
              this.$message.error("类别删除失败:"+res.data.msg);
            }
          }).catch((err)=>{
            console.log(err);
          })
        }).catch(()=>{})
      },
      updateCotegory(c,i){
        this.$prompt("请输入您想修改的类型名：","提示",{inputValue:c.name}).then(({value})=>{
          var url=this.$store.state.globalSettings.apiUrl+"/admin/category";
          this.$axios.put(url,[{cname:value},c.cid]).then((res)=>{
              if(res.data.code==200){
              //数据库添加成功
              this.$message.success("修改成功");
              //模型数据添加
            }else{
              this.$message.error("修改失败："+res.data.msg);
            }
          }).catch((err)=>{
            console.log(err);
          })
        }).catch(()=>{})
      }
    }
  }
</script>


