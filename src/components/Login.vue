<template>
  <div class="login">
    <el-card class="box-card">
      <template #header>
        <div class="clearfix">
          <span>学生管理系统</span>
        </div>
      </template>
      <el-form label-width="60px" :model="form" ref="form" :rules="rules">
        <el-form-item label="用户名" prop="username">
          <el-input v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" v-model="form.password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="login('form')">登录</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>
<script>
import { nameRule, passRule } from '../utils/vaildate.js'
import { setToken } from '@/utils/setToken.js'
import { login } from '@/api/api.js'
export default {
  data() {
    return {
      form: {
        username: "",
        password: "",
      },
      rules: {
        username: [{ validator: nameRule, trigger: "blur" }],
        password: [{ validator: passRule, trigger: "blur" }],
      },
    };
  },
  methods: {
    login(form) {
      this.$refs[form].validate((valid) => {
        if (valid) {
          console.log(this.form);
           /*  把这个登录房费封装成api进行调用
           this.service.post('/login', this.form)
            .then((res) => {
                if (res.data.status === 200) {
                    setToken('username', res.data.username)
                    setToken('token', res.data.token)
                    this.$message({message: res.data.message, type: 'success'})
                    this.$router.push('/home')
                }
            })
            */
           login(this.form).then(res => {
                if (res.data.status === 200) {
                    setToken('username', res.data.username)
                    setToken('token', res.data.token)
                    this.$message({message: res.data.message, type: 'success'})
                    this.$router.push('/home')
                }
           })
        } else {
          console.error(this.form);
        }
      });
    },
  },
};
</script>
<style lang="less">
.login {
  width: 100%;
  height: 100%;
  position: absolute;
  background: url('../assets/bg.png') center no-repeat;
  .el-card {
      background: #65768557;
  }
  .el-button{
    width: 300px!important;
  }
  .box-card {
    width: 450px;
    margin: 200px auto;
    color: #fff;
    .clearfix{
      font-size: 25px;
      color: #2c3e50;
    }
    .el-input{
      width: 300px;
    }
    .el-form .el-form-item__label {
        color: #fff;
    }

    .el-card__header {
      font-size: 34px;
    }
    .el-button {
      width: 100%;
    }
  }
}
</style>
