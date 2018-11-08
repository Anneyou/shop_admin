<template>
  <div class="login">
    <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      <img src="../assets/avatar.jpg" alt="">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="form.username" placeholder="请输入用户名"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="form.password" type="password" placeholder="请输入密码"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="loginForm('form')" :plain="true">登录</el-button>
        <el-button @click="resetForm('form')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '用户名不能为空', trigger: 'blur' },
          { min: 3, max: 9, message: '长度在 3 到 9 个字符', trigger: 'blur' }
        ],
        password: [{ required: true, message: '密码不能为空', trigger: 'blur' }]
      }
    }
  },
  methods: {
    loginForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          axios({
            url: 'http://localhost:8888/api/private/v1/login',
            method: 'post',
            data: this.form
          }).then(res => {
            if (res.data.meta.status === 200) {
              console.log(res.data)
              this.$message({
                type: 'success',
                showClose: true,
                message: '登录成功'
              })
              localStorage.setItem('token', res.data.data.token)
              this.$router.push({
                path: '/home'
              })
            } else {
              this.$message({
                showClose: true,
                message: '用户名或密码错误',
                type: 'error'
              })
            }
          })
        } else {
          console.log('验证不通过')
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>

<style lang="less">
.login {
  height: 100%;
  width: 100%;
  background-color: #2d434c;
  overflow: hidden;
  .el-form {
    width: 400px;
    margin: 200px auto;
    padding: 80px 45px 15px;
    background-color: #fff;
    position: relative;
    border-radius: 20px;
    img {
      position: absolute;
      left: 50%;
      top: -80px;
      transform: translateX(-50%);
      border-radius: 50%;
      border: 10px solid #fff;
    }
    .el-button + .el-button {
      margin-left: 80px;
    }
  }
}
</style>
