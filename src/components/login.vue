<template>
<el-form ref="form" :model="form" label-width="80px">
  <el-form-item label="用户名">
    <el-input v-model="form.username"></el-input>
  </el-form-item>
  <el-form-item label="密码">
    <el-input v-model="form.password"></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="login">登陆</el-button>
    <el-button @click="alert">取消</el-button>
  </el-form-item>
</el-form>
</template>

<script>
export default {
  data() {
    return {
      form:{
        username: '',
        password: ''
      }
    }
  },
  methods: {
    login() {
       // 获取已有账号密码
       this.$http.get('/api/login/getAccount').then(
         (response) => {
           console.log(response)
           let params = {
             username: this.form.username,
             password: this.form.password
           }
           return this.$http.post('/api/login/createAccount', params)
         }
       ).then(
         (response) => {
           console.log(response)
         }
       ).catch(
         (reject) => {
           console.log(reject)
         }
       )
       }
  }
}
</script>
<style lang="css">
</style>
