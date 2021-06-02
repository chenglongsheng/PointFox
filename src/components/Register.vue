<template>
  <div id="registerWrapper">
    <div id="registerContent">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>用户注册</span>
          <span id="tips" v-text="tips" :class="this.tipsState?this.greenTips:this.redTips" ></span>
        </div>
        <div id="registerContentBody">
            <el-input id="newusername" v-model="newUser.newusername" placeholder="请输入账号" @blur="checkUserInform" clearable></el-input>
            <el-input id="newpassword" v-model="newUser.newpassword" placeholder="请输入密码" show-password clearable></el-input>
        </div>
        <div id="registerSubmit">
            <el-button id="systemSubmit" @click="registerSystem" :disabled="!this.tipsState" v-if="this.tipsState">注册</el-button>
            <el-button id="disabledSystemSubmit" @click="registerSystem" :disabled="!this.tipsState" v-else>注册</el-button>
        </div>
      </el-card>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
export default {
  name:"register",
    data(){
        return {
            newUser:{
                newusername:"",
                newpassword:""
            },
            tips:"",
            tipsState:true,
            redTips:"redTips",
            greenTips:"greenTips"

        }
    },
    methods:{
      // 检查用户信息
      checkUserInform(){
        // alert("焦点离开")
        Axios.get('http://localhost:3000/register/check',{
          params:this.newUser
        }).then((reaponse) =>{
          this.tips = (reaponse.data.message)
          if(reaponse.data.state == "success"){
            this.tipsState = true;
          }
          else{
            this.tipsState = false;
          }
          
        })
      },


      // CORS 
      registerSystem(){
        // Axios.get('http://localhost:3000/register',{
        //     params:this.newUser
        // }).then(function(response){
        //   alert(response.data)
        // })
        Axios.post("http://localhost:3000/register",{
          params:this.newUser
        }).then(function(response){
          alert(response.data.message)
        });
      }
    }
};
</script>

<style>
#registerWrapper {
  /* background-color: purple; */
  height: 100%;
}
#registerContent{
    width: 400px;
    margin:0 auto;
    padding-top: 100px;
}
#systemSubmit{
    display: block;
    margin: 0 auto;
    background-color: #0a9588;
    color:white;
    border-color: #0a9588;
}
#disabledSystemSubmit{
    display: block;
    margin: 0 auto;
    background-color: gray;
    color:white;
    border-color: gray;
}
#newpassword{
  margin-top:5px;
  margin-bottom: 8px;
}
#tips{
  margin-left:  15px;
  /* color: blue; */
}
.redTips{
   color: red;
}
.greenTips{
  color:green;
}
</style>
