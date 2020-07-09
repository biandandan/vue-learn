<template>
  <div class="h2">欢迎你</div>
</template>

<script>
export default {
  name: 'Welcome',
  data () {
    return {
      login_form: {
        username: '',
        password: ''
      }
    }
  },
  created () {
    this.getCookie()
  },
  destroyed () {
    // const nowTime = new Date()
    // this.clearCookie()
  },
  methods: {
    /**
     * @description: 方法描述
     * @param {String} username
     * @param {String} password
     * @param {Number} num 到期天数
     */
    setCookie (username, password, num) {
      const time = new Date()
      time.setTime(time.getTime() + 24 * 60 * 60 * 1000 * num)
      // 编码，不让cookie明文显示
      window.document.cookie = `username: ${window.btoa(username)}; 
                                path: /; 
                                expires: ${time.toGMTString()}`
      window.document.cookie = `password: ${window.btoa(password)}; 
                                path: /; 
                                expires: ${time.toGMTString()}`
    },
    /**
     * Read cookie
     * window.atob() base64解码
     */
    getCookie () {
      // console.log(document.cookie)
      if (document.cookie.length > 0) {
        const cookieArr1 = document.cookie.split(';')
        for (let i = 0; i < cookieArr1.length; i++) {
          const cookieArr2 = cookieArr1[i].split(':')
          for (let j = 0; j < cookieArr2.length; j++) {
            // 避免输入中含有空格，消除文本框空格
            cookieArr2[j] = cookieArr2[j].trim()
            if (cookieArr2[j] === 'username') {
              // 读取并解码
              this.login_form.username = window.atob(cookieArr2[1])
            } else {
              this.login_form.password = window.atob(cookieArr2[1])
            }
          }
        }
        console.log('cookie')
        console.log(this.login_form)
      }
    },
    // Clear cookies
    clearCookie () {
      this.setCookie('', '', -1)
    }
  }
}
</script>

<style scoped>

</style>
