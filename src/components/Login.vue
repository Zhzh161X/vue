<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatar_box">
        <img class="login_img"
             src="../assets/logo.png"
             alt="">
      </div>
      <!-- 表单区域 -->
      <el-form :model="loginForm"
               label-width="0px"
               class="login_form"
               :rules="loginFormRules"
               ref="loginFormRef" 
               
               >
               <!--ref="loginFormRef"表单实例对象 -->
        <!-- 用户名 -->
        <el-form-item label="" prop="username">
          <el-input v-model="loginForm.username"
                    prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item label="" prop="password">
          <el-input v-model="loginForm.password"
                    prefix-icon="iconfont icon-3702mima"
                    type="password"></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      //这是登录表单的数据绑定对象
      loginForm: {
        username: '',
        password: '',
      },
      //表单验证规则对象
      loginFormRules: {
        //验证用户名是否合法
        username: [
           { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 5 个字符', trigger: 'blur' },
        ],
        //验证密码是否合法
        password: [
         { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' },
        ],
      },
    }
  },
  methods:{
    //重置登录表单
    resetLoginForm(){
      // console.log(this)
      this.$refs.loginFormRef.resetFields();
    },
    login(){
      this.$refs.loginFormRef.validate(async valid=>{
        // console.log(valid)
        if(!valid) return;
        const {data:res} = await this.$http.post('login',this.loginForm);
        if(res.meta.status !==200) return this.$message.error('登录失败！')
        this.$message.success('登录成功！')
        // console.log(res)
        // 1.将登陆成功之后的token，保存到客户端的sessionStorage中
        // 1.1 项目中出来登录之外的其他API接口，必须在登录之后才能访问
        //1.2 token只应在当前网站打开期间生效，所以将token保存在sessionStorage
        window.sessionStorage.setItem('token',res.data.token);
        //1.3 通过编程式导航跳转到后台主页，路由地址是/home
        this.$router.push('/home');
      })
    }
  }
}
</script>

<style>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
}
.avatar_box {
  height: 130px;
  width: 130px;
  border: 1px solid #eeee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
}
.login_img {
  height: 100%;
  width: 100%;
  border-radius: 50%;
  background-color: rgba(170, 243, 213, 0.667);
}
.btns {
  display: flex;
  justify-content: flex-end;
}
.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
</style>
