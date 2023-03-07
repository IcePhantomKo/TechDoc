### 创建 Express 服务方法
1. install node => cnpm install node
1. install express => cnpm install express
1. 


#### Demo Code (app.js) 
```
const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser')

const app = express();

app.use(bodyParser.urlencoded({
    extended:true
}));
app.use(bodyParser.json());
app.use(cors());

app.get('/',(req,res)=>{
    res.send('Welcome');
})
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
