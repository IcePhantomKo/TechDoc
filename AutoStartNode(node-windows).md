### Node项目开机自启动
1. 在需要自启动的项目中安装 node-windows 模块 npm install node-windows --save
2. 在项目根目录创建nw.js文件
代码如下
```
/**
 * @description: node自启动功能,node nw.js 开启，再次启动为uninstall
 */
let Service = require('node-windows').Service;

let svc = new Service({
	name:'node_liveroom',
	description: '服务',
	script:'./app.js',	//node项目要启动的文件路径
	wait:'1',		//程序崩溃后重启的时间间隔
	grow:'0.25',		//重启等待时间成长值，比如第一次为1秒，第二次为1.25秒
	maxRestarts:'40'	//60秒内最大重启次数
});

// 监听安装事件
svc.on('install',()=>{
	svc.start();
	console.log('Install complete');
});

// 监听卸载事件
svc.on('uninstall',()=>{
	console.log('Uninstall complete');
	console.log('The service exists: ', svc.exists);
})

// 防止程序运行两次 
svc.on('already installed',()=>{
	console.log('The service is already installed');
})

// 如果在就卸载
if(svc.exists) return svc.uninstall();

// 不存在就安装
svc.install();
```

3. 运行nw.js文件  命令：node nw.js 这个时候如果安装了安全管家等软件会阻止，直接允许就可以了。运行成功后在电脑的服务中就能看到这个服务，现在就可以像普通的windows-server服务一样操作。

4. 运行node nw.js命令后，会在启动的node文件对应的目录下生成daemon文件夹（包括普通日志，错误日志，配置信息等）。

5. 现在就可以连接nodejs项目，Nodejs项目开机自启动基本已完成。再次运行 node nw.js命令会卸载掉我们安装的服务。

资源整理自：https://www.bbsmax.com/A/mo5kQwvEzw/
