<template>
  <div class="login">
    <div class="logo-set">
      <img src="http://viptail.image.alimmdn.com/files/official_web/img/icon_logo.png" alt="">
    </div>
     <div class="md-example-child md-example-child-input-item-1">
      <md-field>
        <md-input-item
          v-model="user.name"
          title="用户名"
          placeholder="用户名"
          is-title-latent
          clearable
        ></md-input-item>
        <md-input-item
          v-model="user.password"
          title="密码"
          placeholder="密码"
          is-title-latent
          clearable
          type="password"
          ></md-input-item>
      </md-field>
      <md-button type="primary" round @click="handleClick">登录</md-button>
      <p style="margin-top:20px">密码忘记了？<span class="pass-back">请找回密码</span></p>
      <!-- 微信登录 -->
      <div style="margin-top:30px">
        <div class="title">
            <span class="caption"><span class="line line-l"></span>或<span class="line line-r"></span></span>
        </div>
        <img src="../../../static/img/wechat.png" alt="" style="height:30px">
      </div>
      <!-- 跳转注册页面 -->
     <div class="bottom-set">
       <p style="text-align:center">还没有账号？<span class="pass-back" @click="goRegister">注册</span></p>
     </div>
    </div>
  </div>  
</template>
<script>
import { mapActions } from 'vuex'
import {getLogin} from '@/api/user'
import { Toast } from 'mand-mobile'
export default {
  name: 'Login',
  data () {
    return {
      user:{
        name:'',
        password:'',
      }
      }
  },
  mounted () {
  },
  methods: {
    ...mapActions(['setUser']),
    handleClick(){
      let param = {
        uName :this.user.name,
        pWord :this.user.password
      }
      this.$http.post('/user', param).then(res => {
        if(res.data.code== 200){
            let User = JSON.stringify(res.data.data);
            //登录信息存到本地
            this.setData('user',User);
            //存到vuex
            this.setUser(this.user);
            Toast.succeed(`欢迎回来, ${this.user.name}`,1500)
            this.$router.push({path:'/'});       
         }
      })
    },
    // 跳转到注册页面
    goRegister() {
      console.log(111)
      this.$router.push({path:'/register'});  
    }
  }
}
</script>
<style lang="scss" scoped>
.login{
  position: relative;
}
.logo-set{
  margin:30px auto 20px
}
.logo-set img{
  height:40px;
  margin-left: 10px;
}
.md-input-item-input{
  font-size: 20px
}
.md-example-child-input-item-1.md-field{
    padding-bottom:40px;
}
.md-button.primary{
  color:$font-color;
  font-size: 20px; 
  height: 50px;
  background-color:$topic-color
}
.title { position: relative; z-index: 2; font-size: 16px; line-height: 24px; text-align: center; color: #999; overflow: hidden; }
.title .caption { position: relative; display: inline-block; }
.title .caption .line { position: absolute; top: 11px; width: 600px; height: 1px; background-color: #ddd; }
.title .caption .line-l { right: 100%; margin-right: 15px; }
.title .caption .line-r { left: 100%; margin-left: 15px; }
.pass-back{
  color:$topic-color
}
.login .md-action-bar{
  margin:0;
  padding: 0;
}
.bottom-set{
  position: fixed;
  bottom: 0px;
  width: 100vw;
}
.bottom-set p{
  font-size:15px;
}
</style>
