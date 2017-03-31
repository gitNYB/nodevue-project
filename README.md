# nodevue-project
###  STEP 1: Vue-cli

```shell
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
 - install ElementUI and  vue-resource
 ```shell
 npm i element-ui -D
 npm i vue-resource -D
 cd src
 vi main.js
 ```
 ```js
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
import VueResource from 'vue-resource'
Vue.use(ElementUI)
Vue.use(VueResource)
 ```
 
 - form for login
   - input for username
   - input for password
   - button for event   
```shell
 cd src/components/
 #creat login.vue 
``` 
  
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
```shell
cd src
#edit main.js
#add UI组件
```
```js
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
Vue.use(ElementUI)
```
```shell
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
###  STEP 3: Node Server
```shell
mdkir server
cd server
#add index.js,db.js,api.js
npm i express –D
npm i body-parser -D
npm i mongoose -D
```
- index.js
```js
//引入编好的api
const api = require('./api')
//引入文件模块
const fs = require('fs')
//引入处理路径的模块
const path = require('path')
//引入处理post数据的模块
const bodyParser = require('body-parser')
//引入Express
const express = require('express')
const app = express()

app.use(bodyParser.json())
app.use(bodyParser.urlencoded({extended:false}))
app.use(api)
//访问静态资源文件，这里访问所有dist目录下的静态资源文件
app.use(express.static(path.resolve(__dirname,'../dist')))
//因为是单页面应用，所有请求都走/dist/index.html
app.get('*',function(req,res){
  const html = fs.readFileSync(path.resolve(__dirname,'../dist/index.html'),'utf-8')
  res.send(html)
})
//监听8081端口
app.listen(8081)
console.log('success listen 8081...')
```
- db.js
```js
const mongoose = require('mongoose');
// 连接数据库 如果不自己创建 默认test数据库会自动生成
mongoose.connect('mongodb://localhost/test');

// 为这次连接绑定事件
const db = mongoose.connection;
db.once('error',() => console.log('Mongo connection error'));
db.once('open',() => console.log('Mongo connection successed'));
/************** 定义模式loginSchema **************/
const loginSchema = mongoose.Schema({
    username : String,
    password : String
});

/************** 定义模型Model **************/
const Models = {
    Login : mongoose.model('Login',loginSchema)
}

module.exports = Models;
```
- api.js
```js
// 可能是我的node版本问题，不用严格模式使用ES6语法会报错
"use strict";
const models = require('./db');
const express = require('express');
const router = express.Router();

/************** 创建(create) 读取(get) 更新(update) 删除(delete) **************/

// 创建账号接口
router.post('/api/login/createAccount',(req,res) => {
    // 这里的req.body能够使用就在index.js中引入了const bodyParser = require('body-parser')
    let newAccount = new models.Login({
        username : req.body.username,
        password : req.body.password
    });
    // 保存数据newAccount数据进mongoDB
    newAccount.save((err,data) => {
        if (err) {
            console.log(err);
            res.send(err);
        } else {
            console.log('createAccount successed');
            res.send('createAccount successed');
        }
    });
});
// 获取已有账号接口
router.get('/api/login/getAccount',(req,res) => {
    // 通过模型去查找数据库
    models.Login.find((err,data) => {
        if (err) {
            res.send(err);
        } else {
            res.send(data);
        }
    });
});

module.exports = router;
```
```shell
cd config
#edit index.js
```
- /config/index.js
```js
proxyTable: {
      '/api': {
          target: 'http://localhost:8081/api/',
          changeOrigin: true,
          pathRewrite: {
            '^/api': ''
          }
        }
    }
 ```

