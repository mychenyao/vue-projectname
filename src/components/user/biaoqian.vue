<template>
      <div class="biaoqian">
        <div class="biaoqianShowdn">

        </div>
         <div class="biaoqian_box">
                <div class="biaoqian_boxTop">打标签
                </div>

                 <div class="biaoqian_number">
                   <ul>
                     <li v-for="(item,index) in biaoqianArray"><span @click="spanClick(item,index)"
                        :class="{spanActive:spanIndex == index}"></span>
                        <input maxlength=15 minlength=1 type="text" v-model="item.name" @blur.prevent="blurClick(item,index)"/>
                        <span @click="shanchu(item,index)">X</span>
                        <span>({{item.serviceNumber}})</span>
                     </li>
                   </ul>

                 </div>
                 <div class="biaoqian_center">
                   <el-input type="text" placeholder="请输入标签名字" v-model="biaoqianName" style="width:15%; margin:2% 2% 1% 2%; height: 38%;"></el-input>
                   <el-button @click="yesClick" style="width:10%; height: 45%;">确定</el-button>
                 </div>
                 <div class="biaoqian_footer">
                    <div>
                      <span @click="OneYesClick">确定</span>
                      <span @click="quxiao">取消</span>
                    </div>

                 </div>
         </div>

      </div>
</template>
<script>
//  import {mapState,mapMutations,mapGetters,mapActions} from 'vuex'
  export default {
    props:{
      str:String    //要传的id
    },
    data(){
      return{
        biaoqianName : "",  //输入的标签
        biaoqianArray : [],//显示的标签
        spanIndex:-1,  //选中的下标
        lableID : "",  // 单选选中的id

      }
    },
    created(){

      let url = common.apidomain+"/userLabel/findlistUserLabel";
      this.$http.get(url).then((res)=>{
        this.biaoqianArray = res.data.result
      })
    },
    methods:{

      yesClick(){ //确定的click
        let reg = /^[\u4E00-\u9FA5A-Za-z0-9_]{1,15}$/ig;
           if(this.biaoqianName === ""){
               return this.$queryFun.successAlert.call(this,"标签不能为空")
           }else if(reg.test(this.biaoqianName)){

               var objParms ={}
               objParms.name = this.biaoqianName;
               let url = common.apidomain+"/userLabel/saveUserLabel";
               this.$http.post(url,objParms).then((res)=>{
                   if(res.data.code === "0000"){
                       alert("添加成功")
                           let url = common.apidomain+"/userLabel/findlistUserLabel";
                            this.$http.get(url).then((res)=>{
                              console.log(res)
                              this.biaoqianArray = res.data.result
                              console.log(this.biaoqianArray)
                            })
                   } else{
                             this.biaoqianName = "";
                               return this.$queryFun.successAlert.call(this,res.data.error);

                           }
                       }).catch((error)=>{
                          console.log(error)
                       })
                }else{
                   this.biaoqianName= "";
                   return this.$queryFun.successAlert.call(this," 输入必须为1-15个中文、英文、数字包括下划线")
           }
      },
      OneYesClick(){
        if(this.spanIndex == -1){
          return this.$queryFun.successAlert.call(this,"请选择标签")
        }else{
          let objParmOne = {};
          objParmOne.userIds =this.str;
          objParmOne.labelId =this.lableID;
          let url = common.apidomain+"/userInfo/createUserLabel";
          this.$http.post(url,objParmOne).then((res)=>{
            if(res.data.code === "0000"){
              this.$queryFun.successAlert.call(this,"添加成功","1")
            }else{
              return this.$queryFun.successAlert.call(this,res.data.error)
            }
          }).catch((error)=>{
            this.$queryFun.successAlert.call(this,error)  //弹框
          })
             this.$emit("fouShow",false);
          setTimeout(()=>{
            location.reload();

          },500)

        }
      },
      spanClick(v,i){
         this.spanIndex = i;
         this.lableID = v.id;
      },
      shanchu(v,i){
        let shanchuObj = {}
        shanchuObj.state = "1";
        shanchuObj.id = v.id;
        let url = common.apidomain+"/userLabel/updateUserLabel";
         this.$http.post(url,shanchuObj).then((res)=>{
           if(res.data.code === "0000"){
             console.log(res)
             this.$queryFun.successAlert.call(this,"标签删除成功","1")
                let url = common.apidomain+"/userLabel/findlistUserLabel";
                      this.$http.get(url).then((res)=>{
                        this.biaoqianArray = res.data.result
                 })
           }

          }).catch((error)=>{
                console.log(error)
          })
      },
      blurClick(v,i){
//        alert("我是去焦点了")
        let jiaodianOnj = {};
        jiaodianOnj.id = v.id;
        jiaodianOnj.name = v.name;
         let url = common.apidomain+"/userLabel/updateUserLabel";
         this.$http.post(url,jiaodianOnj).then((res)=>{
           if(res.data.code === "0000"){
             console.log(res)
             let reg = /^[\u4E00-\u9FA5A-Za-z0-9_]{1,15}$/ig;
//             if(res.test())
             this.$queryFun.successAlert.call(this,"标签修改成功","1")
                let url = common.apidomain+"/userLabel/findlistUserLabel";
                      this.$http.get(url).then((res)=>{
                        this.biaoqianArray = res.data.result

                 })
           }

          }).catch((error)=>{
                console.log(error)
          })

      },
      quxiao(){
         this.$emit("fouShow",false)

      }
    }
  }

</script>

<style scoped lang="scss">
  .biaoqian{
    position: absolute;
    left: 0;
    top:0;
    bottom: 0;
    right: 0;
    z-index:9999;
    .biaoqianShowdn{
      width: 100%;
      height: 100%;
      background: black;
      opacity: 0.3;
    }
    .biaoqian_box{
     font-family: MicrosoftYaHei;
      font-size: 14px;
      overflow: hidden;
      letter-spacing: 0;
      width:60%;
      border-radius: 10px;
      position:absolute;
      left:50%;
      top:50%;
      transform:translate(-50%,-50%);
      background:#fff;
      height:70%;
      .biaoqian_boxTop{
        width: 100%;
        height: 15%;
        text-align: center;
        line-height:300%;
        font-size: 30px;
        background: #ECECEC;
        position: relative;
      }
      .biaoqian_center{
        width:100%;
        height:13%;
        input{
          width:40%;
          margin:2% 4% 1% 2%;
          height: 45%;
          border: 0;
          border: 1px solid #cccccc;
        }
        button{
          background: #ffffff;
          width:10%;
          height: 45%;
          border: 0;
          border: 1px solid #cccccc;
        }
      }
      .biaoqian_number{
        width:96%;
        height:58%;
        padding: 2%;
        overflow-y: auto;
        ul{
          width:100%;
          li{
              width:25%;
              float: left;
            margin-bottom: 20px;
            span{
              color: black;
            }
          }
          span:nth-child(1){
            display: inline-block;
            width: 10px;
            height: 10px;
            border: 1px solid black;
          }
          span:nth-child(1).spanActive{
            background: #279447;
          }

          input{
            display: inline-block;
            width:50%;
            margin:0 5px;
            border: 0;
          }
          span:nth-child(3){
            // margin-right: 5px;
          }
        }
      }
      .biaoqian_footer{
        width:100%;
        height: 20%;
        div{
          width:40%;
          margin-left: 30%;
          height:45%;
          margin-top:1.5%;
          border-radius:5px;
          span{
            display: inline-block;
            width: 50%;
            cursor: pointer;
            line-height: 44px;
            box-sizing: border-box;
            float: left;
            text-align: center;
            background: #279447;
            color: #ffffff;
            border-radius:2px;
          }
          span:nth-child(1){
            background: #fff;
            color: black;
            border: 1px solid #cccccc;
          }
        }
      }
    }


  }

</style>

