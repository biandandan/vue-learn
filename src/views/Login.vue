<template>
  <div class="login-container">
      <div class="login-table">
        <!-- 顶部logo -->
        <div class="logo">
          <img src="./../assets/img/logo.png" alt="MEDIC">
        </div>
        <!-- 表单 -->
        <div class="login-form">
          <el-form ref="login_form_ref" :model="login_form" :rules="login_rules">
            <!-- 用户名 -->
            <el-form-item prop="username">
              <el-input v-model="login_form.username" prefix-icon="el-icon-user-solid" clearable></el-input>
            </el-form-item>
            <!-- 密码 -->
            <el-form-item prop="password">
              <el-input type="password" @keypress.enter.native="login" v-model="login_form.password" prefix-icon="el-icon-lock" clearable show-password></el-input>
            </el-form-item>
            <!-- 按钮组 -->
            <div class="login-btn-container">
              <el-button type="primary" size="small" @click="login">登录</el-button>
              <el-button type="info" size="small" @click="resetForm">重置</el-button>
            </div>
          </el-form>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      login_form: {
        username: 'admin',
        password: '123456'
      },
      login_rules: {
        username: [
          { required: true, message: '用户名不能为空', trigger: 'blur' },
          { min: 4, max: 16, message: '用户名长度应在4-16之间', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'blur' },
          { min: 6, max: 20, message: '密码长度应在6-20之间', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      const that = this
      this.$refs.login_form_ref.validate((valid) => {
        if (valid) {
          that.$http.post('login', that.login_form)
            .then(resp => {
              const { data: res } = resp
              if (res.meta.status !== 200) {
                that.$message.error(res.meta.msg)
                return
              }
              that.$message.success('登录成功！')
              // 保存用户信息
              const userInfo = res.data
              console.log('userInfo')
              console.log(userInfo)
              window.sessionStorage.setItem('token', userInfo.token)
              that.setCookie(1)
              // 跳转主页
              that.$router.push('/home')
            })
        } else {
          return false
        }
      })
    },
    resetForm () {
      this.$refs.login_form_ref.resetFields()
    },
    /**
     * @description: 方法描述
     * @param {Number} num 到期天数
     */
    setCookie (num) {
      const time = new Date()
      const username = this.login_form.username
      const password = this.login_form.password
      // 过期时间（添加有效日期（UTC 时间）。 默认情况下，在浏览器关闭时会删除 cookie）
      time.setTime(time.getTime() + 24 * 60 * 60 * 1000 * num)
      // 编码，不让cookie明文显示
      window.document.cookie = `username: ${window.btoa(username)}; 
                                path: /; 
                                expires: ${time.toGMTString()}`
      window.document.cookie = `password: ${window.btoa(password)}; 
                                path: /; 
                                expires: ${time.toGMTString()}`
    }
  }
}
</script>

<style lang="less" scoped>
  .login-container {
    width: 100%;
    height: 100%;
    background: #2b2b2b;
  }
  .login-table {
    background: white;
    width: 440px;
    padding: 20px;
    border-radius: 10px;
    position: absolute;
    left: 50%;
    top: 40%;
    transform: translate(-50%, -50%);
    .logo {
      width: 50%;
      position: relative;
      left: 50%;
      transform: translate(-50%);
      img {
        width: 100%;
      }
    }
    .login-form {
      width: 95%;
      margin: 20px auto 0;
      .login-btn-container {
        display: flex;
        justify-content: flex-end;
      }
    }
  }
</style>
