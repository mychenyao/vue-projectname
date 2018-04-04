<template>
      <div class="biaoqian">
        <div class="biaoqianShowdn">

        </div>
         <div class="biaoqian_box">
                <div class="biaoqian_boxTop">打标签</div>
                <div class="biaoqian_center">
                      <input type="text" placeholder="请输入标签名字" v-model="biaoqianName">
                       <button @click="yesClick">确定</button>
                </div>
                 <div class="biaoqian_number">
                   <ul>
                     <!--<li v-for="(item,index) in biaoqianArray"><el-checkbox></el-checkbox><span>{{item.name}}</span><span>X</span><span>({{item.number}})</span></li>-->
                     <li><el-checkbox></el-checkbox><span>你好</span><span>X</span><span>(0)</span></li>
                     <li><el-checkbox></el-checkbox><span>你好</span><span>X</span><span>(0)</span></li>
                     <li><el-checkbox></el-checkbox><span>你好</span><span>X</span><span>(0)</span></li>
                     <li><el-checkbox></el-checkbox><span>你好</span><span>X</span><span>(0)</span></li>
                     <li><el-checkbox></el-checkbox><span>你好</span><span>X</span><span>(0)</span></li>
                     <li><el-checkbox></el-checkbox><span>你好</span><span>X</span><span>(0)</span></li>

                   </ul>

                 </div>
                 <div class="biaoqian_footer">
                    <div>
                      <span>确定</span>
                      <span>取消</span>
                    </div>
                 </div>
         </div>
      </div>
</template>
<script>
  export default {
    data(){
      return{
        biaoqianName : "",  //输入的标签
        biaoqianArray : [],//显示的标签
      }
    },
    methods:{
      yesClick(){
        this.biaoqianArray.push(this.biaoqianName);
        let str = this.biaoqianArray.join(",")
        let url = common.apidomain+"/userLabel/saveUserLabel?name="+str;
          this.$http.post(url).then((res)=>{
             console.log(res)
        })
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
      width:60%;
      height: 70%;
      background: #ffffff;
      position: absolute;
      left: 20%;
      top:20%;
      border: 1px solid black;
      .biaoqian_boxTop{
        width: 100%;
        height: 15%;
        border-bottom:1px solid black ;
        text-align: center;
        line-height:300%;
        font-size: 30px;
        background: #cccccc;
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
              /*background: red;*/
              float: left;
            margin-bottom: 20px;
            span{
              color: black;
            }
          }

          span:nth-child(2){
            display: inline-block;
            width:60%;
            margin:0 5px;
          }
          span:nth-child(3){
            margin-right: 5px;
          }
        }
      }
      .biaoqian_footer{
        width:100%;
        height: 20%;
        div{
          width:40%;
          margin-left: 30%;
          border: 1px solid #2c3e50;
          height:45%;
          margin-top:1.5%;
          span{
            display: inline-block;
            width: 50%;
            box-sizing: border-box;
            float: left;
            height: 100%;
            text-align: center;
            line-height: 400%;
            background: #cccccc;
            color: black;
          }
          span:nth-child(1){
            border-right: 1px solid #2c3e50;

          }
        }
      }
    }


  }

</style>

