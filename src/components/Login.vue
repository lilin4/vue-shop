<template>
  <div>
    <!-- 背景板 -->
    <div class="login_cantainer">
      <!-- 登录框 -->
      <div class="login_box">
        <!-- 头像区域 -->
        <div class="avaver_box">
          <img src="../assets/logo.png" alt="" />
        </div>
        <!-- 登陆表达区域 -->
        <el-form
          :model="loginForm"
          :rules="loginFormRule"
          ref="loginFormRef"
          label-width="0px"
          class="login_form"
        >
          <!-- 用户名 -->
          <el-form-item prop="username">
            <el-input
              v-model="loginForm.username"
              prefix-icon="iconfont icon-user"
            ></el-input>
          </el-form-item>
          <!-- 密码 -->
          <el-form-item prop="password">
            <el-input
              v-model="loginForm.password"
              prefix-icon="iconfont icon-3702mima"
            ></el-input>
          </el-form-item>
          <!-- 按钮 -->
          <el-form-item class="login_buts">
            <el-button type="primary" @click="login">登陆</el-button>
            <el-button type="info" @click="resetLoginForm">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 登陆表单的数据绑定对象
      loginForm: {
        username: '',
        password: ''
      },
      // 登陆表单的验证规则对象
      loginFormRule: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入登陆名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入登陆密码', trigger: 'blur' },
          { min: 3, max: 15, message: '长度在 3 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 重置登陆按钮
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields()
    },
    // 登陆按钮
    login() {
      this.$refs.loginFormRef.validate(async (valid, obj) => {
        // 校验失败退出
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登陆失败')
        this.$message.success('登陆成功')
        // 将token存入sessionStorage
        window.sessionStorage.setItem('token', res.data.token)
        // 跳转页面
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
/* 背景板 */
.login_cantainer {
  background-color: #2b4b6b;
  position: absolute;
  height: 100%;
  width: 100%;
}
/* 登录框 */
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
/* 图标 */
.avaver_box {
  width: 130px; /* 长 */
  height: 130px; /* 高 */
  padding: 10px; /* 内边界 */
  border: 1px solid #eee; /* 边框样式:solid 圆 */
  border-radius: 50%; /* 边框角度 */
  box-shadow: 0 0 10px #ddd; /* 外部圆环 */
  position: absolute; /*  定位*/
  left: 50%;
  /* top: 50%; */
  transform: translate(-50%, -50%); /* 水平移动 */
  background-color: #fff;
  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}
/* 按钮样式 */
.login_buts {
  display: flex; /* 弹性模型 */
  justify-content: flex-end; /* 横轴右对齐 */
}
.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
</style>
