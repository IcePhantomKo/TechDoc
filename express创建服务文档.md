### 创建 Express 服务方法
##### 1. Install node: `npm install node`
`node -v` 来确认node安装成功
##### 2. 在项目目录下安装express框架
`npm install express`

#### 3. 在目录下创建app.js文件，代码如下 
```
// 导入express模块
const express = require('express');
// 导入cors解决跨域问题模块
const cors = require('cors');
// 导入bodyParser模块
const bodyParser = require('body-parser')

const app = express();

app.use(bodyParser.urlencoded({
    extended:true
}));
app.use(bodyParser.json());
app.use(cors());

app.listen(8001,()=>{
    console.log('运行在 http://192.168.1.123:8001/');
})
```
##### 4. 启动服务
`node app.js`

### 接口说明
#### 1. 简易接口

直接在创建的app.js中添加接口
```
// GET请求例子
app.get('/',(req,res)=>{
    res.send('Welcome');
})
```

打开浏览器，输入`localhost:8001`将看到Welcome

#### 2. 将接口从app.js中分离出来
##### 2.1 根目录中创建router 和router_handler 两个文件夹
##### 2.2 两个文件夹中分别创建admin.js文件
##### 2.3 在根目录app.js中添加
```
const adRouter = require('./router/admin')
app.use('/admin',adRouter)
```

##### 2.4 编辑router/admin.js
```
// Admin 权限下的路由
const express = require('express')
// 创建路由对象
const adRouter = express.Router()
// 导入用户路由处理函数对应模块
const adminHandler = require('../router_handler/admin')
// 跳转主页接口
adRouter.get('/index',adminHandler.index)

// 将路由分享出去
moduel.exports = adRouter
```
##### 2.5 编辑router_Handler/admin.js
```
// GET('/admin/index')请求跳转主页接口
exports.index = (req,res) =>{
    res.send({
        status:200,
        message: '登录成功'
    })
}
```
##### 2.6 测试接口
打开浏览器输入`localhost:8001/index` 将返回 {"status":200,"message":"hi"}


