### 创建 Express 服务方法
##### 1. Install node: `cnpm install node`
##### 2. Install express: `cnpm install express`

#### 在目录下创建app.js文件，代码如下 
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

// GET请求例子
app.get('/',(req,res)=>{
    res.send('Welcome');
})
// POST请求 localhost:8001/subEvent 接口请求
app.post('/subEvent',(req,res)=>{
    res.send({
        statusCode:200,
    })
    console.log(req.body);
    console.log('当前时间： ' + Date());
})
 
app.listen(8001,()=>{
    console.log('运行在 http://192.168.1.123:8001/');
})
```

##### 3. Start server：`node app.js`
