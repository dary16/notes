###使用node.js搭建服务器显示html页面

首先要搭建好环境，安装node.js,配置好环境变量
第一种方式：express
- 初始化node项目：npm init
- 安装express：npm install express
- 在根目录创建server.js文件

	```javasacsript
	const express = require('express');
	const path = require('path');
	const app = express();

	app.use(express.static(path.join(__dirname, 'public')));

	app.listen(8800, () => {
	    console.log('服务启动了！');
	})
	```
- 将需要访问的html文件放到public文件下（根目录创建）
- 运行：node server.js
- 打开浏览器，在地址栏中输入：http://localhost:8080/index.html就可以看到显示结果了
