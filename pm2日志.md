### 使用pm2生成日志
1. 新建app.json文件，添加以下配置
```
{
  "script": "app.js",
  "instances": 1,
  "error_file": "./splitLog/错误信息.log",
  "out_file": "./splitLog/日志信息.log",
  "log_date_format": "YYYY-MM-DD hh:mm:ss"
}
```
2. 使用`pm2 start app.json`启动项目
* splitLog下生成 错误信息.log 
* splitLog下生成 日志信息.log 
