### 1. 创建 Express 服务方法
##### 1.1 Install node: `npm install node`
`node -v` 来确认node安装成功
##### 1.2 在项目目录下安装express框架
`npm install express`

##### 1.3 在目录下创建app.js文件，代码如下 
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
##### 1.4 启动服务
`node app.js`

### 2. 接口说明

##### 2.1 简易接口

直接在创建的app.js中添加接口
```
// GET请求例子
app.get('/',(req,res)=>{
    res.send('Welcome');
})
```

打开浏览器，输入`localhost:8001`将看到Welcome

##### 2.2 将接口从app.js中分离出来
##### 2.3 根目录中创建 __router__ 和 __router_handler__ 两个文件夹
##### 2.4 两个文件夹中分别创建 __admin.js__ 文件
##### 2.5 在根目录app.js中添加
```
const adRouter = require('./router/admin')
app.use('/admin',adRouter)
```

##### 2.6 编辑router/admin.js
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
##### 2.7 编辑router_Handler/admin.js
```
// GET('/admin/index')请求跳转主页接口
exports.index = (req,res) =>{
    res.send({
        status:200,
        message: '登录成功'
    })
}
```
##### 2.8 测试接口
打开浏览器输入`localhost:8001/index` 将返回 {"status":200,"message":"hi"}

### 3. CORS 相关（跨域问题）
##### 3.1 使用 cors 中间件解决跨域问题
- 运行 npm install cors 安装中间件
- 使用 const cors = require('cors') 导入中间件
- 在路由之前调用 app.use(cors())

##### 3.2 什么是 CORS?
cors（Cross-Origin Resource Sharing,跨域资源共享）由一系列HTTP响应头组成，这些HTTP响应头决定浏览器是否组织前端JS代码跨域获取资源。
##### 3.3 CORS 的注意事项
- 主要在服务器端进行配置。客户端浏览器无需做任何额外配置，即可开启了CORS的接口。
- CORS在浏览器中由兼容性。只有支持XMLHttpRequest Level2的浏览器可以兼容。

##### 3.4 CORS 响应头部 -Access-Control-Allow-Origin,origin 参数的值指定了 **允许访问该资源的外域 URL**

```
res.setHeader('Access-Control-Allow-Origin','http://baidu.com')
```
只接收百度发来的请求

```
res.setHeader('Access-Control-Allow-Origin','*')
```
允许任何域的请求

 ##### 3.5 CORS 响应头部 -Access-Control-Allow-Methods
 默认情况下，CORS 仅支持客户端发起的 GET, POST, HEAD 请求
如果客户端希望通过 PUT DELETE 等方式请求服务器的资源，则需要在服务器端，通过Access-Control-Allow-Methods来指明实际请求所允许使用的HTTP方法
