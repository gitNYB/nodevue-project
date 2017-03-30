# nodevue-project
###  STEP 1: 

```
  vue init webpack nodevue-project   
  #..  no ESlint,no tests  ..
  cd nodevue-project
  npm install
  npm install vue-resource --save-dev
  #启动测试  
  npm run dev  
  #访问 8080端口 
```  
###  STEP 2: view  创建Vue页面
 - form for login
   - input for username
   - input for password
   - button for event
```
 cd src/components/
``` 
  creat login.vue 
```html
<template>
<el-form ref="form" :model="form" label-width="80px">
  <el-form-item label="用户名">
    <el-input v-model="username"></el-input>
  </el-form-item>
  <el-form-item label="密码">
    <el-input v-model="password"></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="onSubmit">登陆</el-button>
    <el-button>取消</el-button>
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
  }
}
</script>
```
```
cd src
#edit main.js
#add UI组件
```
```js
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
Vue.use(ElementUI)
```
```
cd src/router
#edit index.js
```
```js
import login from '@/components/login'
export default new Router({
  routes: [
    {
      path: '/home',
      name: 'Hello',
      component: Hello
    },
    {
      path: '/login',
      name: 'login',
      component: login
    }
  ]
})
```
