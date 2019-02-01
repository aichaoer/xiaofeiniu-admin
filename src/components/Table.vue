<template>
  <div class="xfn-table-info">
    <el-card shadow="hover">
      <div class="xfn-table" :style="{background:getTableColor(data.status)}">{{data.tid}}号桌：{{data.status | tableStatus}}</div>
      <el-button type="success" plain size="small" @click="showTableDetail">详情</el-button>
      <el-button type="danger" plain size="small">修改</el-button>
    </el-card>
    <!--桌台详情对话框-->
    <el-dialog :title="data.tid+'号桌台详情'" :visible="dialogTableDetailVisible" :before-close="closeDialogTableDetailVisible">
      <!--主体-->
      <el-tabs type="border-card" @tab-click="makeQrcode">
        <el-tab-pane label="桌台详情">详情加载中……</el-tab-pane>
        <el-tab-pane label="桌台二维码">
          <!-- <canvas id="qrcanvas"></canvas> -->
          <img :src="qrcodeData">
        </el-tab-pane>
      </el-tabs>
      <!--footer-->
      <div slot="footer">
        <el-button type="primary" @click="dialogTableDetailVisible=false">确定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
  export default{
    data(){
      return {
        dialogTableDetailVisible:false,
        qrcodeData:""
      }
    },
    props:['data'],
    methods:{
      getTableColor(status){
        if(status==1) return "#67C23A";
        else if(status==2) return "#E6A23C";
        else if(status==3) return "#F56C6C";
        else return "#909399";
      },
      showTableDetail(){
        // console.log(this.data);   //当前桌子的数据
        this.dialogTableDetailVisible=true;
      },
      closeDialogTableDetailVisible(){
        this.dialogTableDetailVisible=false;
      },
      makeQrcode(){
        //创建二维码
        //注意：此方法不能再当前组件的mounted中调用，因为绘图必需的canvas在el-dialoog中，，对话框在组件加载时并不在DOM树上
        var qrcode=require("qrcode");
        // var canvas=document.getElementById("qrcanvas");
        //每个桌子对应的url形如：http://127.0.0.1:8092/#/3  3为当前桌子编号
        var tableUrl=this.$store.state.globalSettings.appUrl+"/#/"+this.data.tid;
        //把绘制得到的图片二进制数据转化为Base64编码的字符串
        qrcode.toDataURL(tableUrl,{width:300,errorCorrectionLevel:"H"},(err,url)=>{this.qrcodeData=url})
        // qrcode.toCanvas(canvas,tableUrl,{width:300,errorCorrectionLevel:"H"});
      }
    }
  }
</script>
<style lang="scss">
  .xfn-table-info{
    padding:2px;
    text-align:center;
    .xfn-table{
      width:95%;
      height:100px;
      line-height:100px;
      border:1px solid #aaa;
      border-radius:50%;
      box-shadow:3px -4px 5px #666;
      margin:5px auto;
      #qrcanvas{
      }
    }
  }
</style>


