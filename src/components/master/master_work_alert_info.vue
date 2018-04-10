<template>
  <div id="app">
    <div class="container" v-if="!mapData.isShow">
      <h3 class="alert_title list_name">{{detailedIsShow.title}}
        <p class="closeX" id="closeX" @click="close(detailedIsShow)" v-if="!mapData.isShow"></p>
        <p class="closeX" id="closeX" @click="close(mapData)" v-if="mapData.isShow"></p>
      </h3>
      <div class="scrollbar content">
        <div class="left_info">
          <div class="title">
            师傅业务信息
            <div class="position_icon" @click="isPosition">
                 <i></i>
              查看师傅位置
            </div>
          </div>
          <ul>
            <li>师傅姓名：{{dataObj.name}}</li>
            <li>师傅手机号：{{dataObj.phoneNum|placeholderTel}}</li>
            <li>服务城市：{{dataObj.city|placeholder}}</li>
            <li>师傅类别： {{dataObj.type|mastertype}}</li>
            <li>所属网点：{{dataObj.site||"无"}}</li>
            <li>挂起次数：{{dataObj.hangSize}}</li>
            <li>返修数：{{dataObj.reworkSize}}</li>
            <li>完成保内单：{{dataObj.reworkCompleteSize}}</li>
            <li>服务态度（分）：{{dataObj.serviceAttitude|placeholder}}</li>
            <li>技术能力（分）：{{dataObj.technicalCompetence|placeholder}}</li>
            <li>准时到达（分）：{{dataObj.deviation|placeholder}}</li>
            <li>近期登陆 ：{{dataObj.loginTime|moment("YYYY-MM-DD HH:mm:ss")|placeholder}}</li>
            <li>状态：{{dataObj.state|childState}}</li>

            <li>服务区域：{{areas|listshow}}</li>
            <li>师傅编号：{{dataObj.id}}</li>
            <li>服务工种: {{skills|listshow}}
            </li>
            <li>工作状态:{{dataObj.workState|showWorkState}}</li>
            <li>总接单量:{{dataObj.allOrderCount|placeholder}}</li>
            <li>已完成:{{dataObj.completeCount|placeholder}}</li>
            <li>待服务:{{dataObj.waitService|placeholder}}</li>
            <li>异常次数:{{dataObj.abnormalHangUp|placeholder}}</li>
            <li>确认数据:{{dataObj.abnormalMoney|placeholder}}</li>
            <li>平均分:{{dataObj.applauseRate|placeholder}}</li>
            <li>准时率:{{dataObj.punctualityRate|placeholder}}</li>
            <li>完工率:{{dataObj.completeRate|placeholder}}</li>
            <li>投诉工单:{{dataObj.complainOrder|placeholder}}</li>
          </ul>
        </div>
      </div>
      <div class="consume_info">
      </div>
    </div>
    <div class="container" v-if="mapData.isShow">
      <position-map style="z-index: 999999"  :mapData="mapData"></position-map>
    </div>

    <div class="btn" v-if="!mapData.isShow">
      <button @click="close(detailedIsShow)">关闭</button>
    </div>
    <div class="btn" v-if="mapData.isShow">
      <button @click="close(mapData)">关闭</button>
    </div>
  </div>
</template>
<script>
  import positionMap from "./position"
  export default {
    props:["detailedIsShow"],
    components:{
      positionMap
    },
    data(){
      return{
        mapData:{
          isShow:false,
          masterId:""
        },
        dataObj:{},
        areas:[],
        skills:[],
      }
    },
    created(){
      this.mapData.masterId= this.detailedIsShow.id;
      let url=this.$apidomain+"/masterinfo/finddetail?id="+this.detailedIsShow.id;
      this.$http.get(url).then(res=>{
        let data = res.data;
        this.dataObj = data.result.masterInfo;

        this.areas = data.result.areas;
        this.skills = data.result.skills;

      })

    },
    methods: {
      isPosition(){
        this.mapData.isShow=true;
      },
      close(isShow){
        isShow.isShow=false;
      },
      getIds(obj){
        let k = "";
        obj.forEach((e,i)=>{
          k += e.id+",";
        });
        k = k.substring(0,k.length-1);
        return k;
      },
    },
    mounted() {

    },
    watch:{

    }
  }
</script>
<style scoped lang="scss">
  @import "../../assets/styles/commButton";
  .el-input, .el-input__inner{
    width:auto;
  }
  .container{
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
    >h3{
      font-family: MicrosoftYaHei;
      font-size: 14px;
      color: #333333;
      padding-left:10px;
      letter-spacing: 0;
    }
    >.content{
      height:80%;
      width:100%;
      overflow-y: auto;
      display:flex;
      >div{
        flex:1;
        >ul{
        }
      }
      >.left_info{
        >.title{
          padding-left:10px;
        }
      }
      .title{
        line-height:30px;
        width:100%;
        background: #ECECEC;
      }
    }
    .consume_info{
      padding-bottom:200px;
      position:relative;
      >.title{
        padding-left:10px;
        line-height:30px;
        width:100%;
        background: #ECECEC;
      }
    }
  }
  ul{
    padding-left:10px;
    list-style:none;
    >li{
      line-height:40px;
    }
  }
  .el-select-dropdown__item{
    line-height: 20px;
  }
  .position_icon{
    float:right;
    margin-right:50%;
    cursor:pointer;
    >i{
      display: inline-block;
      width:16px;
      vertical-align: middle;
      height:16px;
      background:url(../../../static/images/position.png) center center no-repeat;
      background-size:100% 100%;

    }
  }
  .closeX{
    background:url(../../../static/images/del.png) center center no-repeat;
    background-size:100% 100%;
    background-origin: content-box;
  }
</style>

