### 使用pm2生成日志说明文档
##### 1. 安装 pm2 `$ npm install pm2@latest -g`
##### 2. 新建app.json文件，添加以下配置
```
{
  "script": "app.js",
  "instances": 1,
  "error_file": "./splitLog/错误信息.log",
  "out_file": "./splitLog/日志信息.log",
  "log_date_format": "YYYY-MM-DD hh:mm:ss"
}
```

##### 3. 使用 `$ pm2 start app.json`启动项目
* splitLog下生成 错误信息.log 
* splitLog下生成 日志信息.log 

##### 4. 安装pm2-logrotate
* pm2-logrotate是一个pm2的插件，可以对pm2日志进行管理，所以它的运行需要依靠pm2。
* 安装 `pm2 install pm2-logrotate`
* 通过 `pm2 conf pm2-logrotate`可以查看详细的配置

##### 5. pm2-logrotate 具体配置说明
* max_size：（默认值 10M）每个文件最大的存储值，当一个文件的大小超过这个值时，它将会对其进行分割。你可以在最后面指定单位：10G、10M、10K。
* retain：（默认值 30）保留的日志文件个数，比如设置为 30，那么在日志文件数达到 30 个后就会将最早的日志文件删除。
* compress：（默认值 false）是否启用 gzip 压缩处理日志文件。
* dateFormat：（默认格式 YYYY-MM-DD_HH-mm-ss）日志文件名的日期格式。如设置的日志文件名为 out.log，就会自动分割生成 out-YYYY-MM-DD_HH-mm-ss.log 的日志文件。
* workerInterval：（默认 30秒）检查日志大小的时间间隔，最小值为 1。
* rotateInterval：（默认值 0 0 * * *）设置时间定时强制分割日志文件，默认值是 0 0 * * *，意思是每天晚上 0 点分割日志文件。（类似于 Linux 中的 cron 定时任务）
* rotateModule： （默认值 true）是否把 pm2 本身的日志文件也进行分割。

##### 6. 如何设置这些值？
```
// 设置每个文件的最大存储值为 1KB
pm2 set pm2-logrotate:max_size 1k

// 设置保留文件个数为10个
pm2 set pm2-logrotate:retain 10

// 设置分割文件时间
pm2 set pm2-logrotate:rotateInterval "0 0 * * 8"
```
##### 7. pm2 常用命令
* `pm2 start <script_file|config_file>[options]`：启动指定应用，如 `pm2 start index.js --name httpServer`；

* `pm2 stop <name> [options]`：停止指定应用，如 `pm2 stop httpServer`；

* pm2 list：把所有 pm2 启动实例列举出来，注意：pm2 stop 某个项目后，该项目还会存在pm2 list 的列表里面，只是状态是 stop，要想去掉该项目，用 `pm2 delete`；

* `pm2 restart <name> [options]`：重启指定应用，如 `pm2 restart httpServer`；

* `pm2 reload <name>`：重载项目。如果项目没有启动就执行 start；如果项目正在运行中就执行relaod，可以做到 0 秒宕机来加载新代码，生产环境多用 reload 来完成代码更新。

* `pm2 show <name> [options]`：显示指定应用详情，如 `pm2 show httpServer`；

* `pm2 delete <appName> [options]`：删除指定应用，如 `pm2 delete httpServer`，如果修改应用配置行为，最好先删除应用后，重新启动方才生效，如修改脚本入口文件；

* `pm2 kill`：杀掉 pm2 管理的所有进程；

* `pm2 logs <name>`：查看指定应用的日志，同时有标准输出和标准错误，`pm2 logs httpServer --lines 5` 查看最后 5 行日志；

* `pm2 monit`：监控各个应用进程 cpu 和 memory 使用情况；

* `pm2 update`：保存进程列表，退出旧的 PM2 并恢复所有进程，即可以实现 restart 重启进程服务功能或者更新内存中的 PM2 进程（即更新 pm2，更新PM2非常快（少于几秒）并且无缝。）；

* `pm2 save`：保存当前应用列表，相当于获取当前运行的 Node 应用程序的快照.；

* `pm2 resurrect`：重新加载保存的应用列表，相当于恢复快照；

* `pm2 startup`：产生 init 脚本，保持进程活着。即可以设置为开机自启（先 pm2 save 保存当前进程状态，再 pm2 startup 生成开机自启动的命令）。

##### 8. 开机自启动
windows不支持直接 `$ pm2 startup`,需要使用其他库生成自启动脚本
* [pm2-windows-service](https://www.npmjs.com/package/pm2-windows-service)
* [pm2-windows-startup](https://www.npmjs.com/package/pm2-windows-startup)
```
npm install pm2-windows-startup -g
pm2-startup install
```
pm2 将在启动时恢复已保存的进程
  
##### 9. 资源整理自:
* https://blog.csdn.net/u013196396/article/details/86065202
* https://blog.csdn.net/weixin_46560589/article/details/128371807
* https://www.jianshu.com/p/f4e81412d4f2
